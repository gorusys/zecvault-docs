# macOS

ZecVault builds a universal macOS application supporting both Apple silicon (M1/M2/M3/M4) and Intel. The `.dmg` distributes a signed `.app` bundle.

---

## Install from release

Download the latest `.dmg` from [GitHub Releases](https://github.com/gorusys/zecvault/releases):

| File | Architecture |
|---|---|
| `ZecVault_0.1.4_aarch64.dmg` | Apple silicon (M-series) |
| `ZecVault_0.1.4_x64.dmg` | Intel Mac |

1. Open the `.dmg`
2. Drag **ZecVault** to `/Applications`
3. Eject the disk image

### First launch (not yet notarized)

Until code-signing is fully set up, macOS may show a Gatekeeper warning:

1. Right-click the app in Finder
2. Choose **Open**
3. Click **Open** in the dialog

This only needs to be done once. After that, double-click to launch normally.

Alternatively, from Terminal:
```bash
xattr -dr com.apple.quarantine /Applications/ZecVault.app
```

---

## Build from source

### Prerequisites

Install Xcode command line tools:
```bash
xcode-select --install
```

Install Rust (1.87+):
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```

Install Node.js 20+ and pnpm:
```bash
# Using Homebrew (recommended):
brew install node pnpm
# Or via nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install 20 && nvm use 20
npm install -g pnpm
```

### Clone and build

```bash
git clone https://github.com/gorusys/zecvault
cd zecvault
pnpm install
```

**Development:**
```bash
pnpm dev:macos
```

**Production build:**
```bash
pnpm build:mac
```

Output in `apps/macos/src-tauri/target/release/bundle/`:
- `macos/ZecVault.app`
- `dmg/ZecVault_x.x.x_aarch64.dmg` (or `_x64.dmg` on Intel)

---

## Universal binary

To build a fat binary that runs natively on both Apple silicon and Intel:

```bash
rustup target add x86_64-apple-darwin aarch64-apple-darwin
cd apps/macos
pnpm tauri build --target universal-apple-darwin
```

The universal binary is larger (~2×) but works on any Mac without Rosetta.

---

## Notarization

For distribution outside the App Store, Apple requires notarization. Once you have an Apple Developer ID:

```bash
# Notarize the DMG:
xcrun notarytool submit ZecVault_0.1.4_aarch64.dmg \
  --apple-id "your@email.com" \
  --team-id "TEAMID" \
  --password "app-specific-password" \
  --wait

# Staple the notarization ticket:
xcrun stapler staple ZecVault_0.1.4_aarch64.dmg
```

After notarization, Gatekeeper accepts the app without any user override.

---

## Troubleshooting

### Cargo build fails

```bash
cd apps/macos/src-tauri
cargo clean
cd ../../..
pnpm build:mac
```

### WebKit/WebView issues

Tauri on macOS uses the system WebKit (Safari engine). Ensure your macOS is up to date for the best compatibility. No additional WebView installation is needed — it ships with macOS.

### `xcode-select: error: tool 'xcodebuild' requires Xcode`

Install the full Xcode from the App Store (not just command line tools):
```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```
