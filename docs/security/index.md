# Security

ZecVault is non-custodial: your seed phrase and private keys are generated on your device and never leave it. This section explains the full security model — how the app protects your keys, what each layer does, and where the limits are.

---

## Security at a glance

<div class="grid cards" markdown>

-   :material-key-variant: **Encryption model**

    ---

    AES-256-GCM encryption with Argon2id key derivation. Your password is never stored — only used to derive the decryption key on demand.

    [:octicons-arrow-right-24: Encryption](encryption.md)

-   :material-backup-restore: **Backup & recovery**

    ---

    Your 24-word seed phrase is your only true backup. How to store it safely and what to do if you lose your device.

    [:octicons-arrow-right-24: Backup](backup.md)

-   :material-fingerprint: **Biometrics & OS keyring**

    ---

    Touch ID, Windows Hello, Linux Secret Service. How biometric unlock works without weakening your encryption.

    [:octicons-arrow-right-24: Biometrics](biometrics.md)

</div>

---

## What's stored where

| Location | Contents | Secret? |
|---|---|---|
| App data directory (disk) | Encrypted mnemonic + salt + nonce | Mnemonic is AES-256-GCM encrypted |
| App data directory (disk) | Wallet SQLite database | Contains tx history, not keys |
| `localStorage` (browser) | Addresses, balances, vault goals, settings | No secrets — addresses are safe to expose |
| OS keyring | Your wallet password (biometrics only) | Managed by the OS; requires system auth |
| Memory (runtime) | Decrypted mnemonic, spending keys | Never persisted; discarded after use |
| Network | Nothing | Private keys are never transmitted |

Your seed phrase is **never** in `localStorage`, **never** in a log file, and **never** sent over the network.

---

## The threat model

ZecVault is designed to protect against:

- ✅ Remote attackers who compromise your files (encrypted mnemonic)
- ✅ Apps reading `localStorage` (no secrets stored there)
- ✅ Network eavesdroppers (gRPC/TLS + shielded transactions)
- ✅ Lightwalletd server operators (server cannot see your keys or tx contents)

It does **not** protect against:

- ❌ An attacker with physical access to your unlocked device
- ❌ Malware with keylogging capabilities capturing your password
- ❌ A compromised or modified version of the app bypassing vault locks
- ❌ Forgetting your password (no recovery path without the seed phrase)

---

## The vault security model

Vault balances are a **soft lock** — enforced by the app, not the blockchain. The 24-hour break cooldown is a commitment device for self-accountability.

This means:
- A reset or fresh install on a new device starts vault accounting fresh
- A sufficiently determined attacker with device access and your password could bypass vault locks

Design vaults as savings goals you want to protect from your own impulse spending — not as cryptographic escrow for funds you can't afford to lose.
