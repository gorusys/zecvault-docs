# Linux

ZecVault builds and runs on any modern Linux distribution. Pre-built packages are available for Ubuntu/Debian and Fedora/RHEL users. Arch and NixOS users can build from source.

---

## Install from release (recommended)

Download the latest release from [GitHub Releases](https://github.com/gorusys/zecvault/releases).

=== "AppImage"

    No installation required. Works on any x86-64 Linux distro.

    ```bash
    chmod +x ZecVault_0.1.4_amd64.AppImage
    ./ZecVault_0.1.4_amd64.AppImage
    ```

    To integrate with your desktop (app menu, file associations):
    ```bash
    # Install AppImageLauncher (optional)
    # Or manually create a .desktop entry
    ```

=== "Debian / Ubuntu"

    ```bash
    sudo dpkg -i zecvault_0.1.4_amd64.deb
    # Resolve any dependency issues:
    sudo apt-get install -f
    # Launch:
    zecvault
    ```

    Supported: Ubuntu 22.04+, Debian 11+, Pop!_OS, Linux Mint 21+.

=== "Fedora / RHEL"

    ```bash
    sudo rpm -i zecvault-0.1.4-1.x86_64.rpm
    # Or with dnf:
    sudo dnf install ./zecvault-0.1.4-1.x86_64.rpm
    # Launch:
    zecvault
    ```

    Supported: Fedora 38+, RHEL 9+, openSUSE Leap 15+.

---

## Build from source

### Prerequisites

Install the Rust toolchain (1.87+):
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```

Install Tauri system dependencies:

=== "Ubuntu / Debian"

    ```bash
    sudo apt-get install \
      libwebkit2gtk-4.1-dev \
      libgtk-3-dev \
      libappindicator3-dev \
      librsvg2-dev \
      patchelf \
      build-essential
    ```

=== "Fedora / RHEL"

    ```bash
    sudo dnf install \
      webkit2gtk4.1-devel \
      gtk3-devel \
      libappindicator-gtk3-devel \
      librsvg2-devel \
      patchelf \
      gcc
    ```

=== "Arch Linux"

    ```bash
    sudo pacman -S \
      webkit2gtk-4.1 \
      gtk3 \
      libappindicator-gtk3 \
      librsvg \
      patchelf \
      base-devel
    ```

Install Node.js 20+ and pnpm:
```bash
# Node.js via nvm (recommended):
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
nvm install 20
nvm use 20

# pnpm:
npm install -g pnpm
```

### Clone and build

```bash
git clone https://github.com/gorusys/zecvault
cd zecvault
pnpm install
```

**Development (live reload):**
```bash
pnpm dev:linux
```

**Production installer:**
```bash
pnpm build:linux
```

Output in: `apps/linux/src-tauri/target/release/bundle/`
- `appimage/ZecVault_x.x.x_amd64.AppImage`
- `deb/zecvault_x.x.x_amd64.deb`
- `rpm/zecvault-x.x.x-1.x86_64.rpm`

---

## Troubleshooting

### Stale build cache

If the build fails after moving the repo or updating Rust:
```bash
cd apps/linux/src-tauri
cargo clean
cd ../../..
pnpm build:linux
```

### WebKit missing

If you see `error: failed to run custom build command for 'webkit2gtk-sys'`:
```bash
# Ubuntu/Debian:
sudo apt-get install libwebkit2gtk-4.1-dev
# Fedora:
sudo dnf install webkit2gtk4.1-devel
```

### AppImage won't launch

Check FUSE availability:
```bash
# Ubuntu 22.04+:
sudo apt-get install libfuse2
```

Or run with `--appimage-extract-and-run`:
```bash
./ZecVault_0.1.4_amd64.AppImage --appimage-extract-and-run
```

---

## CI/CD

ZecVault uses GitHub Actions for automated Linux builds. The workflow is at `.github/workflows/release-linux.yml`. It runs on `ubuntu-22.04`, installs all prerequisites, and produces the three installer formats on every tagged release.
