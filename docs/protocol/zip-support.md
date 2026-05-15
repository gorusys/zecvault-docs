# ZIP Standards

ZecVault implements the Zcash Improvement Proposals (ZIPs) relevant to a modern wallet. All protocol-level behavior is delegated to the official [librustzcash](https://github.com/zcash/librustzcash) SDK maintained by the Electric Coin Company.

---

## Implemented ZIPs

### ZIP-32 — Hierarchical Deterministic Wallets

**Purpose:** Deterministic key derivation for shielded Zcash keys, analogous to BIP-32 for Bitcoin.

ZIP-32 defines a hierarchy of keys derived from a single seed phrase. ZecVault uses this for:
- Deriving Orchard and Sapling spending keys from the BIP39 mnemonic
- Supporting multiple accounts per seed (different derivation paths)
- Ensuring that a single backup phrase covers all derived accounts

Multi-account support lets users create separate accounts (with separate addresses) from one seed — one backup restores everything.

---

### ZIP-302 — Standardized Memo Format

**Purpose:** A structured 512-byte encrypted memo field for shielded transactions.

ZIP-302 specifies how the 512-byte memo field in Zcash shielded notes should be encoded and interpreted. ZecVault uses it for:
- User-written payment notes (attached to sends)
- ZV1 vault labels (automated, embedded by the app)
- Future structured data formats

Memos are encrypted inside the shielded note — only the recipient can read them.

---

### ZIP-316 — Unified Addresses

**Purpose:** A single address format that encodes multiple receiver types (Orchard, Sapling, transparent).

A Unified Address lets the sender's wallet automatically choose the best pool it supports. ZecVault generates a Unified Address (with Orchard + Sapling receivers) as the default receive address for every wallet.

Benefits:
- One address works with wallets that support Orchard, Sapling, or both
- As the ecosystem upgrades to Orchard, the address keeps working
- No manual address type management for the user

---

### ZIP-317 — Proportional Transfer Fee Mechanism

**Purpose:** A fee schedule that scales with transaction complexity (number of inputs/outputs) rather than a fixed fee.

ZecVault uses ZIP-317 for all fee calculations. The fee preview step shows the exact fee — computed server-side against the actual transaction structure — before the user commits. Typical fees:

| Transaction type | Approximate fee |
|---|---|
| Simple shielded send | 0.00001 ZEC (1,000 zatoshis) |
| Complex multi-input | Higher (scales with inputs/outputs) |
| Send Max | Automatically deducted from total |

---

### ZIP-321 — Payment Request URIs

**Purpose:** A `zcash:` URI format for encoding a payment request (address, amount, memo) in a QR code or link.

```
zcash:u1exampleaddress...?amount=1.5&memo=base64encodedmemo
```

ZecVault parses ZIP-321 URIs when:
- You scan a QR code on the Send screen
- You tap a `zcash:` link in another app (desktop only)

The Send screen auto-fills the address, amount, and memo from the URI. You can review and edit any field before confirming.

---

## Crates used

All ZIP implementations are provided by the official ECC Rust SDK:

| Crate | ZIPs covered |
|---|---|
| `zcash_client_backend` | ZIP-32, ZIP-316, ZIP-317, sync/spend logic |
| `zcash_keys` | ZIP-32 key derivation |
| `zcash_protocol` | Protocol types, fee primitives |
| `zcash_address` | Address encoding/decoding, ZIP-316 parsing |
| `zip32` | ZIP-32 derivation path types |
