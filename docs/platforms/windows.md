# Windows

ZecVault builds a native Windows application using Tauri. Installers are distributed as `.msi` (Windows Installer) and `.exe` (NSIS). WebView2 is bundled automatically on Windows 10 installations.

---

## Install from release

Download the latest installer from [GitHub Releases](https://github.com/gorusys/zecvault/releases):

| File | Format | Use when |
|---|---|---|
| `ZecVault_0.1.4_x64_en-US.msi` | Windows Installer | Default — most users |
| `ZecVault_0.1.4_x64-setup.exe` | NSIS | Custom install path, older systems |

Run the installer and follow the prompts. ZecVault appears in the Start menu after installation.

### Windows Defender SmartScreen

On first launch you may see:

> "Windows protected your PC — Microsoft Defender SmartScreen prevented an unrecognized app from starting."

Click **More info → Run anyway**. This warning appears for apps without an EV code signing certificate. A signed release is planned for future versions.

---

## Build from source

### Prerequisites

**Visual Studio Build Tools 2022:**

Download from [visualstudio.microsoft.com](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022) and install with:
- ✅ **Desktop development with C++** workload
- ✅ MSVC v143 compiler
- ✅ Windows 10/11 SDK

**Rust (1.87+):**

Download and run [rustup-init.exe](https://win.rustup.rs/). The installer automatically detects Visual Studio and configures the MSVC target.

**WebView2:**

- Windows 11: Pre-installed
- Windows 10: Bundled with the installer (bootstrapped automatically)
- For build machines: Install [WebView2 Evergreen Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/)

**Node.js and pnpm:**

Download Node.js 20+ from [nodejs.org](https://nodejs.org), then:
```powershell
npm install -g pnpm
```

---

### Clone and build

```powershell
git clone https://github.com/gorusys/zecvault
cd zecvault
pnpm install
```

**Development:**
```powershell
pnpm dev:windows
```

**Production installer:**
```powershell
pnpm build:win
```

Output in `apps\windows\src-tauri\target\release\bundle\`:
- `msi\ZecVault_x.x.x_x64_en-US.msi`
- `nsis\ZecVault_x.x.x_x64-setup.exe`

---

## Code signing

Unsigned Windows installers trigger SmartScreen. For a trusted release, sign with an **Authenticode code signing certificate**:

```powershell
# Sign the MSI (requires signtool.exe from Windows SDK):
signtool sign /tr http://timestamp.sectigo.com /td sha256 /fd sha256 /a ZecVault_0.1.4_x64_en-US.msi
```

For WinGet submission, the MSI must be signed. For Microsoft Store (MSIX), additional packaging is required via `msixpackagingtool` or WiX.

---

## Troubleshooting

### Build fails: `LINK : fatal error LNK1181: cannot open input file`

Ensure Visual Studio Build Tools are installed with the MSVC toolchain:
```powershell
rustup target add x86_64-pc-windows-msvc
```

### Cargo clean

```powershell
cd apps\windows\src-tauri
cargo clean
cd ..\..\..
pnpm build:win
```

### WebView2 not found

On older Windows 10 systems without WebView2:
1. Install [WebView2 Evergreen Bootstrapper](https://go.microsoft.com/fwlink/p/?LinkId=2124703)
2. Run the installer as Administrator
3. Retry launching ZecVault

### antivirus false positives

Some antivirus software flags newly compiled Rust binaries. If your AV quarantines the build output, add an exclusion for `apps\windows\src-tauri\target\` during development.
