---
hide:
  - navigation
  - toc
---

# ZecVault

<div class="hero-section" markdown>

<div class="hero-text" markdown>

## Save with purpose. Spend with privacy.

ZecVault is a non-custodial Zcash wallet built around savings goals. Set a target, give it a name, and deposit toward it — your keys never leave your device.

[Get Started](getting-started/index.md){ .md-button .md-button--primary }
[Download](getting-started/installation.md){ .md-button }

</div>

</div>

---

## Why ZecVault?

<div class="grid cards" markdown>

-   :material-shield-lock:{ .lg .middle } **True Non-Custody**

    ---

    Your seed phrase and private keys are generated entirely on your device. No accounts. No servers. No cloud sync. Only you can access your funds.

    [:octicons-arrow-right-24: Security model](security/index.md)

-   :material-eye-off:{ .lg .middle } **Shielded by Default**

    ---

    ZecVault uses Zcash's Orchard and Sapling shielded pools. Transaction amounts and participants are cryptographically private — invisible to chain observers.

    [:octicons-arrow-right-24: Privacy features](privacy/index.md)

-   :material-piggy-bank:{ .lg .middle } **Built-in Savings Goals**

    ---

    Create named vaults for your goals — a trip, an emergency fund, new tech. Track progress, build streaks, and stay accountable with a 24-hour break cooldown.

    [:octicons-arrow-right-24: How vaults work](features/vaults.md)

-   :material-devices:{ .lg .middle } **Cross-Platform Native**

    ---

    One codebase, native desktop apps on Linux, macOS, and Windows — plus a web app and installable PWA. Runs on your hardware, not ours.

    [:octicons-arrow-right-24: Platform guides](platforms/index.md)

</div>

---

## What makes ZecVault different

### Privacy is the default, not an add-on

Most wallets make you opt into privacy. ZecVault defaults to shielded Unified Addresses — every deposit lands in Zcash's Orchard pool, invisible to outside observers. Transparent addresses are available when you need them (exchanges, legacy integrations), but the wallet actively guides you toward privacy.

### Savings goals backed by real cryptography

Vault balances aren't just bookkeeping. When you deposit into a vault via a Zcash transaction, the wallet embeds a signed, encrypted memo (`ZV1:<vaultId>:<goalName>`) inside the shielded note. That memo travels with the transaction on-chain — encrypted, unreadable by anyone but you — so your goals survive device restores and multi-device use.

### Zero-knowledge sync — no full node needed

ZecVault connects to a lightwalletd server over encrypted gRPC/TLS. You get full shielded wallet functionality — scan, receive, send — without storing 50+ GB of blockchain history. The server never sees the contents of your transactions.

### Built on the ECC Rust SDK

The wallet backend uses the same [librustzcash](https://github.com/zcash/librustzcash) libraries that Zcash core maintains. ZIP-32, ZIP-302, ZIP-316, ZIP-317, ZIP-321 — all implemented correctly, not reimplemented from scratch.

---

## Feature overview

| Feature | Description |
|---|---|
| BIP39 24-word seed | Full entropy wallet generation, industry-standard recovery |
| Unified Addresses | One address, Orchard + Sapling receivers, widest compatibility |
| Savings vaults | Named goals with targets, deadlines, streaks, break cooldown |
| Round-up savings | Auto-capture ZEC from sends, round to nearest threshold |
| Multi-wallet | Multiple seeds, multiple accounts per seed (ZIP-32) |
| Shield transparent funds | Sweep exchange deposits into your shielded balance |
| Sapling → Orchard migration | Self-send to upgrade funds to the modern pool |
| Fee preview | See exact fees before committing any transaction |
| OS keyring / biometrics | Touch ID, Windows Hello, Linux Secret Service |
| Send Max | Drain all shielded funds in a single transaction |
| Payment URIs | ZIP-321 `zcash:` links with amount and memo support |
| Encrypted memos | 512-byte ZIP-302 memos, shielded inside transactions |
| 4-word alias | Human-readable address verification for Receive screen |
| Birthday height | Skip history before wallet creation for fast initial sync |
| Custom lightwalletd | Point to your own server for maximum metadata privacy |

---

## Quick start

=== "Desktop"

    ```bash
    # Requires Rust 1.87+ — see Installation for system deps
    git clone https://github.com/gorusys/zecvault
    cd zecvault
    pnpm install
    pnpm dev:linux      # or dev:macos / dev:windows
    ```

=== "Web"

    ```bash
    git clone https://github.com/gorusys/zecvault
    cd zecvault
    pnpm install
    pnpm dev            # http://localhost:5173
    ```

!!! tip "Browser mode"
    Running `pnpm dev` launches the full UI with mock wallet data. You can explore every screen without a native build.

---

## Current version

**ZecVault v0.1.4** — active development. Core wallet functionality is complete. Mobile and browser extension scaffolds are in progress.

[View on GitHub](https://github.com/gorusys/zecvault){ .md-button }
