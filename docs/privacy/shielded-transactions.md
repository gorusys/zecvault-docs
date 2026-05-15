# Shielded Transactions

Zcash pioneered cryptographic transaction privacy using **zero-knowledge proofs** (zk-SNARKs). When you send or receive ZEC via a shielded address, the transaction is recorded on the blockchain — but the amount, sender, and recipient are all hidden.

---

## How it works

A Zcash shielded transaction proves three things to the network using a zk-SNARK:

1. The sender owns the funds being spent (without revealing which funds)
2. No ZEC is created out of thin air (the output ≤ the input)
3. The transaction is authorized by the spending key (without revealing the key)

The proof is verified by every full node, but the transaction data itself — amounts and addresses — is encrypted. Only the recipient (with the appropriate viewing key) can decrypt and read the details.

---

## Orchard vs Sapling

ZecVault supports two shielded pools:

=== "Orchard (recommended)"

    Orchard is Zcash's modern shielded pool, introduced in the NU5 upgrade. It uses the **Halo 2** proving system — no trusted setup required, faster proof generation.

    - Addresses start with `u1` (Unified Address with Orchard receiver)
    - Best forward privacy
    - Default pool for ZecVault receives and sends
    - Some older wallets may not yet support sending to Orchard

=== "Sapling (compatible)"

    Sapling was Zcash's first widely-adopted shielded pool. It provides equivalent on-chain privacy but uses an older proving system (Groth16) with a trusted setup.

    - Addresses: `zs1...` (Sapling-only) or `u1...` (Unified with Sapling receiver)
    - Supported by virtually all Zcash wallets and exchanges
    - ZecVault can migrate Sapling funds to Orchard via Settings

When you share your Unified Address, the sender's wallet automatically picks the best pool it supports — Orchard first, then Sapling as a fallback.

---

## What's private in a shielded transaction

| Information | Shielded (Orchard/Sapling) | Transparent |
|---|---|---|
| Sender address | Hidden | Public |
| Recipient address | Hidden | Public |
| Amount sent | Hidden | Public |
| Transaction memo | Encrypted (512 bytes) | Not available |
| Transaction existence | Public (timing/size visible) | Public |
| Block height | Public | Public |

---

## Transparent transactions

Transparent addresses (`t1...`) offer **no privacy** — they work exactly like Bitcoin. Anyone can see:

- Which address sent funds
- Which address received funds
- The exact amount

ZecVault supports transparent addresses for compatibility with exchanges and legacy systems, but warns you prominently when sending to a transparent address and actively guides you toward shielded alternatives.

!!! info "Shielding transparent funds"
    If you receive ZEC to a transparent address (common from centralized exchanges), ZecVault will offer to shield it into your Orchard pool. After shielding, the funds are indistinguishable from any other shielded balance.

---

## The memo field

Every shielded transaction can carry up to **512 bytes** of arbitrary text in an encrypted memo (ZIP-302). The memo:

- Is encrypted inside the shielded note
- Is readable only by the recipient (using their incoming viewing key)
- Is invisible to lightwalletd, blockchain explorers, and any third party
- Can contain UTF-8 text, base64 data, or structured data like ZV1 vault labels

ZecVault displays memos inline in the transaction history. They are decrypted locally during block scanning — never sent over the network in plaintext.
