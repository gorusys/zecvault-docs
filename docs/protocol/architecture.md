# System Architecture

ZecVault is a two-layer application: a React frontend for all UI and a Rust wallet backend for all cryptography and network communication. On desktop, they communicate through Tauri's IPC bridge. In the browser, the frontend runs alone with mock wallet data.

---

## High-level diagram

```
┌─────────────────────────────────────────────┐
│              React Frontend                  │
│  (screens, state stores, routing, vaults)   │
│                                             │
│  invoke("command", args) → Tauri IPC        │
└───────────────┬─────────────────────────────┘
                │ Tauri IPC (desktop only)
                │ Mock fallback (browser)
┌───────────────▼─────────────────────────────┐
│           Rust Wallet Backend               │
│  (keys, addresses, sync, sign, broadcast)  │
│                                             │
│  gRPC over TLS → lightwalletd              │
└───────────────┬─────────────────────────────┘
                │ gRPC/TLS
┌───────────────▼─────────────────────────────┐
│              lightwalletd                   │
│  (Zcash compact block server)               │
└─────────────────────────────────────────────┘
```

The frontend never handles private keys. All sensitive operations happen in the Rust layer.

---

## Frontend

### Stack

- **React 19** with **TanStack Router** (file-based routing via `apps/web/src/routes/`)
- **Zustand 5** stores with `persist` middleware (localStorage)
- **Tailwind CSS 4** with custom design tokens
- **Radix UI** + custom shadcn/ui components
- **Tauri API 2** for IPC (`invoke`, event listeners)
- **Vite 7** + Cloudflare Workers plugin (web deployment)

### Three Zustand stores

| Store | Contents | Persisted |
|---|---|---|
| `useWalletStore` | Active wallet ID, addresses, balance per pool, tx history, sync state | localStorage |
| `useVaultStore` | Vault goals, contributions, status, streaks | localStorage |
| `useSettings` | Theme, currency, lightwalletd URL, network, feature flags | localStorage |

No secret material is ever written to localStorage. Addresses and balances are not sensitive.

### IPC pattern

Every wallet operation uses `invoke()` from `@tauri-apps/api/core`:

```typescript
// apps/web/src/lib/wallet-native.ts
import { invoke } from "@tauri-apps/api/core";

export async function getBalance(walletId: string) {
  return invoke("get_balance", { walletId });
}
```

In browser mode (no Tauri), `invoke` is shimmed to return empty/mock data — the full UI works without a native build.

---

## Rust Backend

### Source

`apps/linux/src-tauri/src/lib.rs` — identical across all three desktop shells (`linux`, `macos`, `windows`). Only the Tauri config files differ between platforms.

### 31 Tauri Commands

Commands are registered via `#[tauri::command]` and exposed in `tauri::Builder`. Key groups:

**Wallet lifecycle:**
- `wallet_create`, `wallet_restore`, `wallet_finalize_create`
- `wallet_list`, `wallet_set_active`, `wallet_remove`
- `wallet_update_name`, `wallet_update_birthday`
- `wallet_export_backup`, `wallet_export_all_backups`

**Balance & sync:**
- `get_balance` — per-pool balances (Orchard, Sapling, transparent, pending)
- `get_transactions` — full transaction history with decrypted memos
- `start_sync` — triggers compact block download + scan
- `wallet_reconcile_derived_addresses`

**Sending:**
- `preview_transfer` / `propose_transfer` / `execute_transfer` — two-step send
- `preview_send_max` / `send_max_transfer`
- `shield_transparent_funds`
- `migrate_sapling_to_orchard`

**Network:**
- `get_unified_address`, `get_latest_block_height`
- `set_lightwalletd_server`, `get_market_price`

**Lock:**
- `app_get_lock_state`, `app_lock`, `app_unlock`

---

## Sync flow

```
start_sync called
       ↓
Connect to lightwalletd (gRPC/TLS)
       ↓
Fetch latest block height
       ↓
Download compact blocks in batches of 10,000
       ↓
Scan batch for notes belonging to wallet
       ↓
Emit "sync-progress" event → frontend updates progress bar
       ↓
Repeat until current height
       ↓
Emit "sync-complete" → frontend refreshes balance
```

Only one sync runs at a time. A `LIGHTWALLETD_SYNC_LOCK` (async Mutex) serializes concurrent calls to prevent SQLite write conflicts.

---

## Send flow (two-step)

```
User enters recipient + amount + memo
       ↓
preview_transfer called
Backend builds transaction without broadcasting
Returns exact fee + total amount
       ↓
User reviews fee in UI
       ↓
execute_transfer called with the same proposal
Backend signs transaction with spending key
Spending key derived from mnemonic, used once, discarded
       ↓
Transaction broadcast to lightwalletd
       ↓
Next sync picks up the confirmed transaction
```

---

## State persistence

| Store | Where | Notes |
|---|---|---|
| Wallet store | `localStorage` | Addresses, balances, tx cache |
| Vault store | `localStorage` | Goals, contributions, status |
| Settings store | `localStorage` | Preferences, server URL |
| Wallet records | OS app data dir | Encrypted mnemonics (JSON) |
| Wallet DB | OS app data dir | SQLite (zcash_client_sqlite) |

---

## Directory structure

```
apps/
  web/
    src/
      routes/        TanStack Router file-based routes (13 files)
      screens/       Full-page views (12 components)
      stores/        Zustand stores (wallet, vault, settings)
      hooks/         useWallet, use-mobile
      lib/           zec.ts, wallet-native.ts, zcash-address.ts
      components/    Reusable UI (sidebar, vault card, icon, etc.)
  linux/
    src-tauri/
      src/lib.rs     Main Rust backend (3,888 lines, 31 commands)
      Cargo.toml     Rust dependencies
      tauri.conf.json Tauri app configuration
  macos/             Identical to linux/, different tauri.conf.json
  windows/           Identical to linux/, different tauri.conf.json

packages/
  shared/    TypeScript type stubs (shared across apps)
  config/    App ID and constants
  assets/    Asset bundling helpers
```
