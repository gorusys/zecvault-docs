# Transaction History

The **History** screen shows all Zcash transactions associated with your wallet. Filter by type, read encrypted memos, and see which vault each deposit belongs to.

---

## Filters

| Filter | Shows |
|---|---|
| **All** | Every transaction — sent, received, shielded |
| **Vaults** | Only transactions linked to a vault via ZV1 memo |
| **Received** | Incoming transactions |
| **Sent** | Outgoing transactions |
| **Memos** | Transactions that carry a non-empty encrypted memo |

---

## What each transaction shows

- **Date and time** — when the transaction was confirmed on-chain
- **Amount** — in ZEC and your chosen fiat currency
- **Direction** — received (↓) or sent (↑)
- **Address** — counterparty address (or "shielded" if address is hidden)
- **Memo** — decrypted memo text, if present
- **Vault** — vault name if the transaction has a ZV1 memo

---

## Memos

Shielded transactions can carry up to **512 bytes** of encrypted text (ZIP-302). Memos are visible only to the recipient — they travel inside the shielded note and are decrypted when the wallet scans incoming blocks.

Common uses:

- Payment references (invoice numbers, order IDs)
- Personal notes between ZecVault users
- Vault labels (ZV1 memos, written by the app automatically)

Memos are shown inline in the transaction list. Tap any transaction to expand the full memo text.

---

## Vault attribution

When an incoming transaction carries a `ZV1:<vaultId>:<goalName>` memo, the History screen shows the vault name next to that transaction. This is how you can audit which vault received which deposit without opening the Vaults screen.

---

## Privacy note

Transaction history is reconstructed from the Zcash blockchain during sync. The amounts and memos you see are **only visible to you** — the chain records shielded transactions as encrypted blobs. Your transaction history is not exposed to lightwalletd or any third party.

Transparent transactions (sent to or from `t1...` addresses) are an exception: those are fully public on-chain, just like Bitcoin transactions.
