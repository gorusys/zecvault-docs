# Biometrics & OS Keyring

ZecVault supports biometric unlock on all desktop platforms — Touch ID on macOS, Windows Hello on Windows, and the Secret Service (GNOME Keyring / KWallet) on Linux. Biometrics don't weaken your encryption — they're a convenience layer on top of it.

---

## How it works

When you enable biometrics:

1. ZecVault stores your wallet password in the **OS secure credential store**
2. When you unlock, you authenticate with your biometric (fingerprint, face, PIN)
3. The OS authenticates you and returns your password to ZecVault
4. ZecVault uses the password to run Argon2id and decrypt your mnemonic — exactly as if you'd typed it

Your mnemonic is still encrypted with the same AES-256-GCM + Argon2id scheme. Biometrics just store the password for you — securely, in hardware-backed storage that the OS controls.

---

## Platform support

=== "macOS"

    ZecVault uses **Keychain** via the macOS Security framework. Touch ID (and Face ID on Macs with Apple silicon + Apple Vision Pro pairing) authenticate access to the Keychain item. The Keychain is hardware-backed on Macs with the T2 chip or Apple silicon.

    The Keychain item is device-bound — it cannot be exported or synced to iCloud.

=== "Windows"

    ZecVault uses **Windows Credential Manager**. Windows Hello (fingerprint, facial recognition, PIN) authenticates access. On devices with a TPM, credentials are hardware-backed.

    Windows Hello PIN is not the same as your wallet password — it's a second factor that the OS uses to unlock the Credential Manager, which then provides your password to the app.

=== "Linux"

    ZecVault integrates with **Secret Service API** — the standard interface implemented by GNOME Keyring and KWallet. If your desktop environment has one of these running (most do by default), ZecVault will store your password there.

    Biometric authentication on Linux depends on your desktop environment and hardware. If no biometric is configured, the Secret Service still provides a session-locked credential store.

---

## Enabling biometrics

Biometrics are enabled by default during onboarding if your device supports them. To toggle:

**Settings → Security → Biometric unlock**

Disabling biometrics removes your password from the OS keyring. You'll need to enter your password manually on every unlock.

---

## What if biometrics fail?

If biometric authentication fails (hardware issue, finger injury, etc.), ZecVault falls back to **password unlock**. Enter your wallet password and the app unlocks normally.

Your password is the master key — biometrics are always a convenience layer, never a replacement.

---

## Security considerations

- **Biometrics store your password in the OS keyring** — a user with OS admin privileges and a way to extract Keychain/Credential Manager items could theoretically retrieve it. The risk is low on a properly configured personal device.
- **The encrypted mnemonic on disk is still your last line of defense** — even if the keyring is compromised, the attacker still needs to decrypt the mnemonic file.
- **PIN lock** is also available in Settings → Security as an additional app-level lock separate from biometrics.

---

## PIN lock

ZecVault offers an optional **PIN lock** independent of biometrics. Set a 4–8 digit PIN in **Settings → Security → PIN lock**. The PIN is verified locally with Argon2id — it doesn't replace your wallet password but adds a quick-lock layer for shared device scenarios.
