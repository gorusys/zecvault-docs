# Vault Protocol (ZV1)

The ZV1 memo format is ZecVault's protocol for linking on-chain Zcash transactions to savings vault goals. It is a plain-text memo schema embedded in the encrypted ZIP-302 memo field of shielded transactions.

---

## Format

```
ZV1:<vaultId>:<goalName>
```

### Fields

| Field | Type | Description |
|---|---|---|
| `ZV1` | Literal | Protocol identifier and version |
| `vaultId` | String | App-generated vault identifier (e.g., `v3_1a2b3c4d`) |
| `goalName` | String | Human-readable vault name (e.g., `Bali Trip 2026`) |

### Example

```
ZV1:v3_1a2b3c4d:Bali Trip 2026
```

---

## Privacy properties

ZV1 memos are embedded inside **ZIP-302 shielded memo fields**. This means:

- The memo is encrypted inside the shielded note
- Only the recipient (wallet owner) can decrypt and read it
- Blockchain observers, lightwalletd servers, and block explorers cannot see it
- The transaction appears as a normal shielded transfer on-chain

The vault structure is invisible to anyone without the wallet's incoming viewing key.

---

## How reconciliation works

When a ZecVault wallet syncs, it:

1. Downloads compact blocks from lightwalletd
2. Scans each block for notes belonging to the wallet
3. For each incoming shielded note, decrypts the memo field
4. If the memo matches the pattern `ZV1:<id>:<name>`, looks up the vault with that `vaultId`
5. Credits the transaction amount to that vault's contribution list

This process is called **deposit reconciliation** and runs automatically after every sync. It's what allows vault deposits to survive device restores and multi-device use.

---

## Deposit lifecycle

```
User taps "Deposit on-chain" in vault
        ↓
App constructs a Zcash transaction:
  - Recipient: vault's dedicated receive address
  - Amount: user-specified
  - Memo: "ZV1:<vaultId>:<goalName>"
        ↓
Transaction is signed and broadcast via lightwalletd
        ↓
Transaction appears in next block
        ↓
On next sync, wallet scans the block
        ↓
Memo is decrypted → parsed as ZV1
        ↓
Vault balance is credited
```

---

## Manual deposits (no ZV1 memo)

If a user records a vault deposit locally (without broadcasting a transaction), no ZV1 memo is created. This is a **local-only record** and will not survive a device restore.

The distinction:

| Deposit type | On-chain? | ZV1 memo? | Survives restore? |
|---|---|---|---|
| On-chain deposit | Yes | Yes | Yes |
| Manual (local) deposit | No | No | No |
| Round-up savings | No | No | No |

---

## Implementing ZV1 in another wallet

To send a deposit that ZecVault will recognize and link to a vault:

1. Know the vault's `vaultId` and `goalName` (the user will provide these)
2. Construct a shielded Zcash transaction to the vault's receive address
3. Set the memo to `ZV1:<vaultId>:<goalName>` (UTF-8, under 512 bytes)
4. Broadcast normally

ZecVault will reconcile the deposit automatically on next sync.

---

## Implementation reference

| Component | Location |
|---|---|
| Memo formatting | `apps/web/src/lib/zec.ts` → `formatVaultMemo()` |
| Memo parsing | `apps/web/src/lib/zec.ts` → `parseVaultMemo()` |
| Reconciliation | `apps/web/src/stores/index.ts` → `reconcileVaultDepositsFromTxHistory()` |
| Vault store | `apps/web/src/stores/index.ts` → `useVaultStore` |
