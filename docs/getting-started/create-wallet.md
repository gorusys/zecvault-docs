# Create a Wallet

Creating a wallet in ZecVault takes about 5 minutes. The app generates a unique 24-word seed phrase on your device — this phrase is the master key to all your funds.

---

## Before you begin

Have a pen and paper ready, or prepare an offline location where you can securely store 24 words. **Do not take a screenshot or type your seed phrase into any text field or messaging app.**

---

## Onboarding walkthrough

### Step 1 — Name your wallet

Enter a name for this wallet (e.g., "Main Wallet" or "Savings"). The name is local only and never shared.

Set a strong password. This password:

- Encrypts your seed phrase on disk using **AES-256-GCM + Argon2id**
- Is used to unlock the app
- Is **never stored anywhere** — if you forget it, your seed phrase is the only recovery path

!!! tip "Choosing a strong password"
    Use a passphrase of 4–6 random words (e.g., "correct horse battery staple") rather than a short complex password. It's easier to remember and harder to brute-force.

---

### Step 2 — Generate your seed phrase

The app generates a 24-word BIP39 seed phrase using 256 bits of cryptographic entropy. The words are displayed on screen — **write them down now, in order**.

!!! danger "This is your only backup"
    The app will ask you to verify four of the 24 words before continuing. If you can't pass the verification, go back and re-read your words carefully.

---

### Step 3 — Verify your backup

The app shows four blank fields, each labeled with a word number (e.g., "Word 7", "Word 13"). Type the correct word from your written backup into each field.

This step exists to confirm you've written the words down correctly — not to store or transmit them anywhere.

---

### Step 4 — Download backup (optional)

You can download a plaintext `.txt` file containing your seed phrase. This is a convenience copy — it is **not** encrypted.

!!! warning "Keep the backup file offline"
    - Do not upload to cloud storage (Dropbox, Google Drive, iCloud)
    - Do not email it to yourself
    - Store it on an encrypted USB drive or print it out and keep it in a secure physical location

---

## After setup

Once onboarding is complete, your wallet will:

1. Derive your Unified Address (Orchard + Sapling receivers)
2. Begin syncing with the Zcash network via lightwalletd
3. Show your balance dashboard

The first sync may take a few minutes if your wallet has a long history. For a brand-new wallet, sync is nearly instant.

---

## Multiple wallets

ZecVault supports multiple wallets (separate seed phrases) from one app. To add a second wallet, go to **Wallets → Add wallet**. You can switch between wallets from the same menu.

Each wallet has its own seed phrase — keep a separate backup for each.

---

## What's next?

- [Receive your first ZEC](../features/receive.md)
- [Create a savings vault](../features/vaults.md)
- [Understand your security model](../security/index.md)
