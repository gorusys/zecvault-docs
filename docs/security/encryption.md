# Encryption Model

ZecVault uses a strong, well-studied encryption scheme to protect your seed phrase on disk. This page explains each component and why it was chosen.

---

## The scheme

When you create or restore a wallet, the Rust backend:

1. Takes your password as input
2. Generates a **random 32-byte salt**
3. Derives a 32-byte encryption key by running your password + salt through **Argon2id**
4. Generates a **random 12-byte nonce**
5. Encrypts the mnemonic using **AES-256-GCM** with the derived key and nonce
6. Stores the encrypted mnemonic, salt, and nonce in a JSON file via `tauri-plugin-store`

```
password + salt  →  Argon2id  →  256-bit key
mnemonic + key + nonce  →  AES-256-GCM  →  ciphertext + auth tag
```

The plaintext mnemonic exists only in memory during this operation and is immediately discarded.

---

## Why Argon2id?

**Argon2id** is the winner of the Password Hashing Competition (2015) and the current recommended password hashing function per OWASP. It's memory-hard, meaning an attacker can't massively parallelize brute-force attempts using GPUs or ASICs — they're bottlenecked by memory bandwidth.

Compared to alternatives:
- **bcrypt / scrypt** — older; Argon2id provides better resistance to GPU attacks
- **PBKDF2** — fast on GPU; unsuitable for protecting mnemonics

A weak password is still a real risk. Argon2id makes cracking expensive — it doesn't make it impossible if the password is short or predictable.

---

## Why AES-256-GCM?

**AES-256-GCM** (Galois/Counter Mode) provides:
- **Confidentiality** — the mnemonic is encrypted
- **Integrity** — the authentication tag detects any tampering with the ciphertext
- **Authenticity** — an attacker who modifies the file will cause decryption to fail, not silently corrupt the mnemonic

256-bit key size provides a 128-bit security margin even against quantum attacks using Grover's algorithm.

---

## How decryption works

Every time the app needs your mnemonic (to sign a transaction, to export a backup):

1. You enter your password (or the OS keyring provides it via biometrics)
2. The backend reads the salt from disk
3. Runs Argon2id to re-derive the key (same password + same salt = same key)
4. Decrypts and authenticates the ciphertext
5. Uses the mnemonic for the operation, then discards it from memory

**Your password is never stored anywhere.** It's used once per unlock and then released.

---

## Lock screen

When the app locks (timeout or manual lock), it:

- Clears the decrypted mnemonic from memory
- Requires your password (or biometric unlock) to re-derive it

The app stores a **password verifier** (a separate Argon2id hash) to validate your unlock attempt before running the full decryption. This prevents unnecessary decryption attempts with wrong passwords.

---

## Key files on disk

The encrypted wallet data is stored in your OS app data directory:

| Platform | Path |
|---|---|
| Linux | `~/.local/share/zecvault/` |
| macOS | `~/Library/Application Support/zecvault/` |
| Windows | `%APPDATA%\zecvault\` |

Inside you'll find:
- `wallet_<id>.json` — encrypted mnemonic, salt, nonce
- `wallet_<id>.db` — SQLite wallet database (transaction history, not keys)

Both are safe to back up. The JSON file is useless without your password. The SQLite DB contains no private keys.
