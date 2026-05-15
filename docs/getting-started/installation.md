# Download & Install

ZecVault ships as a native desktop app for Linux, macOS, and Windows — and as a web app for browser use.

---

## Desktop apps

Download the latest release for your platform from the [GitHub Releases page](https://github.com/gorusys/zecvault/releases).

=== "Linux"

    | Package | Format | Use when |
    |---|---|---|
    | `.AppImage` | Portable binary, no install | Quick run, any distro |
    | `.deb` | Debian/Ubuntu package | Ubuntu, Debian, Pop!_OS |
    | `.rpm` | RPM package | Fedora, RHEL, openSUSE |

    **AppImage** (recommended for most users):

    ```bash
    chmod +x ZecVault_0.1.4_amd64.AppImage
    ./ZecVault_0.1.4_amd64.AppImage
    ```

    **Debian/Ubuntu:**

    ```bash
    sudo dpkg -i zecvault_0.1.4_amd64.deb
    # Launch from your application menu or:
    zecvault
    ```

=== "macOS"

    Download the `.dmg` file, open it, and drag **ZecVault** to your Applications folder.

    ```
    ZecVault_0.1.4_aarch64.dmg   # Apple Silicon (M1/M2/M3/M4)
    ZecVault_0.1.4_x64.dmg       # Intel Mac
    ```

    On first launch, macOS may show a security dialog because the app is not yet notarized via the App Store. To open it:

    1. Right-click the app in Finder
    2. Choose **Open**
    3. Click **Open** in the confirmation dialog

    This only needs to be done once.

=== "Windows"

    | Package | Format | Use when |
    |---|---|---|
    | `.msi` | Windows Installer | Most users |
    | `.exe` | NSIS installer | Older systems, custom install path |

    Run the installer and follow the on-screen prompts. ZecVault will appear in your Start menu after installation.

    !!! note "Windows Defender SmartScreen"
        You may see a SmartScreen warning on first run. Click **More info → Run anyway**. The app will be code-signed in a future release.

---

## Web app

The web app runs the full ZecVault UI in your browser. In browser mode, wallet operations (sync, send, receive) return mock data — it's ideal for exploring the interface without a native build.

To run it locally:

```bash
git clone https://github.com/gorusys/zecvault
cd zecvault
pnpm install
pnpm dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

!!! info "Web app limitations"
    The web app cannot access native OS features (keyring, file system). Real wallet operations — syncing, sending, receiving — require the desktop app.

---

## Build from source

If you want to build ZecVault yourself, see the platform-specific build guides:

- [Linux build guide](../platforms/linux.md)
- [macOS build guide](../platforms/macos.md)
- [Windows build guide](../platforms/windows.md)

---

## System requirements

| Platform | Minimum | Recommended |
|---|---|---|
| Linux | Ubuntu 22.04 / Fedora 38+ | Ubuntu 24.04 LTS |
| macOS | macOS 12 Monterey | macOS 14 Sonoma |
| Windows | Windows 10 (64-bit) | Windows 11 |
| RAM | 512 MB | 2 GB |
| Disk | 200 MB (app) + wallet DB | 500 MB |
| Network | Required for sync | Broadband |
