# Backup & Recovery

Your 24-word seed phrase is the only true backup of your ZecVault wallet. This page explains how to back it up safely, what happens if you lose it, and how to restore on a new device.

---

## The seed phrase is everything

ZecVault generates a **24-word BIP39 seed phrase** when you create a wallet. From this phrase, every address, every private key, and every piece of wallet data can be regenerated.

- If you lose your device but have the seed phrase → full recovery possible
- If you lose your seed phrase and your device breaks → funds are **permanently unrecoverable**
- If you lose your seed phrase but still have your device → you can still access funds as long as the device works

!!! danger "No recovery without the seed phrase"
    There is no "forgot password" and no customer support that can recover your funds. The seed phrase is your only lifeline. Back it up before you deposit anything significant.

---

## How to back up

### Option 1: Write it down (recommended)

Write all 24 words on paper, in order, with the word numbers. Store the paper:

- In a **fireproof and waterproof safe** if possible
- At a location physically separate from your primary device
- Away from areas at risk of flooding, fire, or theft

Use a **pen**, not a pencil. Pencil fades and smears.

!!! tip "Consider a steel backup"
    For long-term storage, engrave or stamp your seed phrase onto a steel plate. Paper degrades; steel survives house fires. Products like Cryptosteel, Bilodal, or similar provide purpose-built seed phrase storage.

### Option 2: Download backup file (convenience only)

During onboarding, ZecVault offers to download a `.txt` file containing your seed phrase. This is a **plaintext file** — it is not encrypted.

If you download a backup file:

- ❌ Do **not** store it in cloud storage (Google Drive, Dropbox, iCloud, OneDrive)
- ❌ Do **not** email it to yourself
- ❌ Do **not** store it on a device that connects to the internet
- ✅ Store on an **encrypted offline USB drive**, physically secured
- ✅ Print it and store it like cash

### Option 3: Multiple physical copies

For large holdings, store copies in multiple separate physical locations — home safe, safe deposit box, trusted family member. Tradeoff: more copies = more attack surface. Weight this against the risk of a single point of failure.

---

## What the backup covers

| Backed up by seed phrase | Not backed up by seed phrase |
|---|---|
| All ZEC balances | Local-only vault deposit records |
| Transaction history (recovered from chain) | Round-up savings history (local only) |
| All derived addresses | App settings and preferences |
| Encrypted memos (recovered from chain) | Vault names and goals (not on-chain) |

Vault data is partially recoverable: any vault deposit made with a ZV1 memo (via on-chain transaction) is re-linked automatically after a full sync. Vault names and goals are not stored on-chain.

---

## Restoring on a new device

See [Restore a Wallet](../getting-started/restore-wallet.md) for the full walkthrough. In brief:

1. Install ZecVault on the new device
2. Choose **Restore from seed**
3. Enter your 24 words
4. Set a new password
5. Enter your birthday height for faster sync (optional but recommended)
6. Wait for sync to complete

---

## Exporting an additional backup later

After initial setup, you can export your seed phrase again from **Wallet → Export backup**. You'll need to enter your password to authorize the export.

You can also export all wallet backups at once from **Wallet → Export all backups** — useful if you have multiple wallets.
