# Restore a Wallet

If you're moving ZecVault to a new device, recovering from a lost device, or importing a Zcash wallet created in another app, you can restore it using your 24-word seed phrase.

---

## What you'll need

- Your **24-word BIP39 seed phrase**, in the correct order
- Optionally: your **wallet birthday height** (the block height when the wallet was first created)

---

## Restore steps

### 1. Open ZecVault and choose "Restore wallet"

On a fresh install, the onboarding screen offers two options:

- **New seed** — generates a new seed
- **Add account** — enter an existing seed phrase

Select **Add account**.

---

### 2. Enter your seed phrase

Type or paste your 24 words, one per field, in the correct order. The app validates each word against the BIP39 wordlist and shows a warning if a word is not recognized.

!!! tip "Paste support"
    You can paste all 24 words at once if they're space-separated. The app splits and populates each field automatically.

---

### 3. Set a new password

Enter a new password to protect the restored wallet on this device. This does not need to match the password you used on your previous device — the seed phrase is the only thing that matters for recovery.

---

### 4. Enter your birthday height (recommended)

The **birthday height** is the Zcash block height at or before the first time your wallet received funds. Setting it accurately means the app can skip scanning earlier blocks, which dramatically reduces initial sync time.

| If you know... | Enter... |
|---|---|
| Approximate date of first use | Look up the date on [zcashblockexplorer.com](https://zcashblockexplorer.com) and enter a block height 1,000 blocks before it |
| Nothing / restoring very old wallet | Leave blank — the app scans from Sapling activation (~2018) |

!!! info "Default birthday heights"
    - **Mainnet:** 419,200 (Sapling activation)
    - **Testnet:** 280,000

You can update the birthday height later in **Wallet → Birthday height** if you realize you set it too high and some history is missing.

---

### 5. Wait for sync

After restore, the wallet scans the Zcash blockchain for transactions belonging to your addresses. Sync time depends on:

- **Birthday height** — the earlier the birthday, the more blocks to scan
- **Your connection** — syncing uses compact blocks, not full blocks, so it's fast even on modest connections
- **Block history** — a wallet with years of transactions takes longer than a fresh wallet

Progress is shown in the sidebar. You can use most of the app while sync is running.

---

## Restoring vault data

Vaults are reconstructed from on-chain transaction memos. After a full sync, the app scans incoming transactions for `ZV1:` memos and automatically re-links them to vault goals.

!!! note "Manual deposits aren't on-chain"
    If you made local-only vault deposits (without broadcasting a transaction), those won't appear after a restore — they have no on-chain footprint. Only deposits made via Zcash transactions with ZV1 memos are reconciled automatically.

---

## Compatibility

ZecVault uses standard BIP39 mnemonics and ZIP-32 key derivation — the same as Ywallet, Zashi, and other Zcash wallets. Any standard 24-word Zcash seed phrase can be restored here.

!!! warning "12-word seeds"
    Some older wallets or exchanges issue 12-word seeds. ZecVault requires 24 words. If you have a 12-word phrase, it may originate from a non-standard derivation — check with the issuing app before attempting a restore.
