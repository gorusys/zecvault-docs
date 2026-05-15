# Features

ZecVault combines a full-featured Zcash wallet with a savings-goal layer built on top of it. Here's everything the app can do.

---

<div class="grid cards" markdown>

-   :material-safe:{ .lg } **Savings Vaults**

    ---

    Name a goal, set a target, save toward it. Track progress, streaks, and enforce your own commitment with a 24-hour break cooldown.

    [:octicons-arrow-right-24: Vaults](vaults.md)

-   :material-send:{ .lg } **Send ZEC**

    ---

    Two-step send with fee preview. Shield transparent funds. Send Max. Payment URI support.

    [:octicons-arrow-right-24: Sending](send.md)

-   :material-qrcode:{ .lg } **Receive ZEC**

    ---

    Unified Address, QR code, 4-word alias verification. Multiple address types available.

    [:octicons-arrow-right-24: Receiving](receive.md)

-   :material-history:{ .lg } **Transaction History**

    ---

    Filterable view of all transactions. Shielded memos displayed inline. Vault attribution shown.

    [:octicons-arrow-right-24: History](history.md)

-   :material-arrow-up-circle:{ .lg } **Round-Up Savings**

    ---

    Capture the spare change from every send. Auto-deposit the difference into a vault of your choice.

    [:octicons-arrow-right-24: Round-Up](round-up-savings.md)

-   :material-wallet-bifold:{ .lg } **Multi-Wallet**

    ---

    Manage multiple seed phrases from one app. Separate accounts, separate backups, all in one place.

    [:octicons-arrow-right-24: Multi-wallet](../getting-started/create-wallet.md)

</div>

---

## Wallet capabilities at a glance

| Capability | Detail |
|---|---|
| Key generation | BIP39 24-word mnemonic, 256-bit entropy |
| Address types | Unified (u1), Sapling (zs1), Transparent (t1), Orchard-only unified |
| Balance tracking | Per-pool: Orchard, Sapling, transparent, pending |
| Network sync | Compact blocks via lightwalletd (gRPC/TLS), batches of 10,000 |
| Send | Two-step preview → execute, fee preview before broadcast |
| Send Max | Drain all shielded funds in one transaction |
| Shield funds | Sweep transparent → Orchard in one tap |
| Pool migration | Sapling → Orchard self-send via Settings |
| Memos | ZIP-302 encrypted 512-byte memos on shielded sends |
| Payment URIs | ZIP-321 `zcash:` links with amount + memo |
| Multi-wallet | Multiple seeds, multiple ZIP-32 accounts per seed |
| Lock screen | Password + optional biometrics (Touch ID, Windows Hello) |
