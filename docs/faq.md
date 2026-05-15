# Frequently Asked Questions

---

## General

### What is ZecVault?

ZecVault is a non-custodial Zcash wallet with a built-in savings goal system called vaults. You set a goal, name it, deposit ZEC toward it, and the app tracks your progress. Your keys never leave your device.

### Is ZecVault free?

Yes. ZecVault is open-source and free to use. You pay standard Zcash network fees when you send transactions — these go to miners, not to ZecVault.

### Does ZecVault have custody of my funds?

No. ZecVault is fully non-custodial. Your seed phrase and private keys are generated on your device and never transmitted anywhere. We have no servers holding your funds and no way to recover your wallet if you lose your seed phrase.

### What Zcash network does ZecVault use by default?

Mainnet. You can switch to testnet in **Settings → Network** for testing.

---

## Wallet & Keys

### Can I use my existing Zcash seed phrase with ZecVault?

Yes. ZecVault uses standard BIP39 24-word mnemonics and ZIP-32 key derivation — the same as Ywallet, Zashi, and other Zcash wallets. Enter your seed phrase using **Restore from seed** on the onboarding screen.

### Can I use a 12-word seed phrase?

No. ZecVault requires 24-word (256-bit entropy) BIP39 mnemonics. Some older wallets and exchanges issue 12-word phrases — check with the issuing app if you have one.

### What happens if I forget my password?

If you forget your password, your **seed phrase is the only recovery path**. Your password is never stored anywhere — it's used only to decrypt your locally-stored mnemonic. Without the password, the only way to regain access is to restore from your 24-word seed phrase.

### Can I change my wallet password?

Yes. Go to **Settings → Security → Change password**. You'll need your current password to authorize the change.

### Can I have multiple wallets?

Yes. ZecVault supports multiple wallets (separate seed phrases) from one app. Go to **Wallets → Add wallet** to create or restore a second wallet. You can switch between them at any time.

---

## Sync & Network

### How long does the initial sync take?

It depends on your wallet's birthday height (the block when the wallet was first created). For a brand-new wallet, sync is near-instant. For a wallet restored from an old seed with a long history, it can take several minutes. Setting an accurate birthday height dramatically reduces sync time.

### What is a birthday height?

The birthday height is the Zcash block height at or before the wallet's first transaction. ZecVault only scans blocks from this height forward, skipping earlier history. If you're restoring an old wallet and know approximately when it was first used, look up the date on a block explorer and enter the corresponding height.

### What is lightwalletd? Can I trust it?

Lightwalletd is a server that serves compact blocks — small summaries of the Zcash blockchain — to light wallets. ZecVault defaults to `zec.rocks`, a community-operated instance with a strong privacy policy. The server can see which blocks you download and when you sync, but **cannot see the contents of your shielded transactions** — those are cryptographically private. If you need more metadata privacy, you can run your own lightwalletd and point ZecVault to it in **Settings → Network**.

### Why isn't my balance updating?

Try tapping the sync button in the sidebar. If sync fails, check:
1. Your internet connection
2. Whether the lightwalletd server is reachable (Settings → Network → test connection)
3. Whether your wallet's birthday height is correct

---

## Privacy

### Are my transactions private?

When you send and receive via shielded addresses (Unified `u1...` or Sapling `zs1...`), amounts and participants are hidden using Zcash's zero-knowledge proof system. Transparent addresses (`t1...`) offer no privacy — those transactions are fully public on-chain.

ZecVault defaults to shielded receiving and actively guides you toward shielded addresses.

### Can the lightwalletd server see my transactions?

The server can see which blocks you download and which transactions you broadcast (since broadcasts go through it). It cannot see the contents of shielded transactions. Your addresses and private keys are never sent to the server.

### What is a transparent address and should I use one?

A transparent address (`t1...`) works like a Bitcoin address — all transactions are fully public on-chain. Use transparent addresses only when required (some exchanges only send to `t1` addresses). After receiving to a transparent address, use **Send → Shield funds** to move the ZEC into your shielded Orchard balance.

---

## Vaults

### Are vaults on-chain?

No. Vaults are an app-layer accounting system on top of your regular Zcash wallet. The ZEC in your vaults is always in your own wallet — not locked in a smart contract. The vault balance is subtracted from your displayed spendable balance so you don't accidentally spend it.

### What happens to my vault if I reset the app or restore on a new device?

Vault goals and local deposit records are stored in `localStorage` and are not backed up automatically. After a restore:
- Your ZEC balance is fully recovered from the blockchain
- Vault deposits made via on-chain transactions with ZV1 memos are re-linked automatically after sync
- Local-only deposits (manual bookkeeping without a Zcash transaction) are not recovered

For important vault data, always use on-chain deposits (the "Deposit on-chain" option in the vault).

### Is the 24-hour break cooldown enforced by the blockchain?

No. The break cooldown is enforced by the app, not the blockchain. It's a commitment device for self-accountability. A modified app or a restored fresh install could bypass it.

### Can I have multiple vaults?

Yes, as many as you want. Each vault has its own name, target, deadline, and dedicated receive address.

---

## Security

### Where is my seed phrase stored?

Your seed phrase is encrypted with AES-256-GCM (using a key derived from your password via Argon2id) and stored in your OS app data directory. It is never stored in `localStorage`, never written to logs, and never transmitted over the network. The encrypted file on disk is useless without your password.

### Is ZecVault open source?

Yes. The full source code is available on [GitHub](https://github.com/gorusys/zecvault) under an open-source license. You can audit, build, and verify it yourself.

### What happens if ZecVault shuts down?

Because ZecVault is non-custodial and open-source, your funds are always accessible. Your seed phrase works in any compatible Zcash wallet (Zashi, Ywallet, etc.). The lightwalletd server can be replaced with any compatible endpoint or your own instance. You are never dependent on ZecVault's continued operation.

---

## Fees

### How much does it cost to send ZEC?

ZecVault uses **ZIP-317 fee calculation**. Fees scale with transaction complexity. A typical shielded send costs approximately **0.00001 ZEC** (1,000 zatoshis). ZecVault always shows the exact fee in a preview step before you confirm any transaction.

### Does shielding transparent funds cost a fee?

Yes. Shielding is a Zcash transaction (transparent input → shielded output) and costs a standard network fee. ZecVault shows the fee before you confirm.
