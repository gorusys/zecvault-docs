# Privacy

ZecVault is built on Zcash — the only major cryptocurrency with cryptographically guaranteed transaction privacy as the default. This section explains what that means in practice and how the wallet protects your financial privacy.

---

## Privacy at every layer

ZecVault approaches privacy as a systems property, not a single feature:

| Layer | Privacy guarantee |
|---|---|
| **Shielded transactions** | Amounts and participants are hidden using zk-SNARKs |
| **Encrypted memos** | 512 bytes of arbitrary text, readable only by recipient |
| **Unified Addresses** | No repeated address exposure; sender's wallet picks the best pool |
| **Local key storage** | Private keys never leave your device |
| **Encrypted mnemonic** | Seed phrase is AES-256-GCM encrypted on disk |
| **gRPC/TLS transport** | All lightwalletd communication is encrypted in transit |
| **Vault memos** | ZV1 savings labels are encrypted inside shielded notes |

---

## Quick reference

<div class="grid cards" markdown>

-   :material-eye-off: **Shielded transactions**

    ---

    Amounts and counterparties hidden by zero-knowledge proofs. The gold standard for financial privacy.

    [:octicons-arrow-right-24: Learn more](shielded-transactions.md)

-   :material-map-marker: **Address types**

    ---

    Unified, Orchard, Sapling, transparent — what each address exposes and when to use it.

    [:octicons-arrow-right-24: Learn more](address-types.md)

-   :material-server: **Lightwalletd & metadata**

    ---

    What the sync server sees, what it doesn't, and how to run your own for maximum privacy.

    [:octicons-arrow-right-24: Learn more](lightwalletd.md)

</div>

---

## What ZecVault cannot protect

Privacy has limits. ZecVault protects your on-chain financial data, but cannot protect:

- **Network metadata** — your IP address when connecting to lightwalletd (use Tor or a VPN if this matters)
- **Exchange KYC** — if you buy ZEC on an exchange, that exchange knows your identity regardless of what wallet you use
- **Physical device access** — if someone has your device and your password, they can access your wallet
- **Transparent transactions** — any funds on transparent addresses (`t1...`) are fully public on-chain

For most users, ZecVault's default settings provide strong financial privacy. For high-stakes threat models, see [Lightwalletd & metadata privacy](lightwalletd.md).
