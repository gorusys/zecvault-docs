# Platforms

ZecVault runs on Linux, macOS, and Windows as native desktop applications — all from a single shared codebase. A web app and installable PWA are also available.

---

## Platform matrix

| Platform | Status | Install formats | Real wallet backend |
|---|---|---|---|
| Linux | ✅ Supported | `.AppImage`, `.deb`, `.rpm` | Yes |
| macOS | ✅ Supported | `.dmg`, `.app` | Yes |
| Windows | ✅ Supported | `.msi`, `.exe` | Yes |
| Web (browser) | ✅ Supported | — | Mock only |
| PWA (installable) | ✅ Supported | Installed from browser | Mock only |
| iOS | 🔲 Scaffold | — | Planned |
| Android | 🔲 Scaffold | — | Planned |
| Browser extension | 🔲 Scaffold | — | Planned |

---

## Desktop platform guides

<div class="grid cards" markdown>

-   :fontawesome-brands-linux: **Linux**

    ---

    Build prerequisites, AppImage / deb / rpm install, CI/CD with GitHub Actions.

    [:octicons-arrow-right-24: Linux guide](linux.md)

-   :fontawesome-brands-apple: **macOS**

    ---

    Xcode prerequisites, universal binary, notarization, App Store notes.

    [:octicons-arrow-right-24: macOS guide](macos.md)

-   :fontawesome-brands-windows: **Windows**

    ---

    Visual Studio prerequisites, MSI/NSIS installer, WebView2, code signing.

    [:octicons-arrow-right-24: Windows guide](windows.md)

</div>

---

## Shared codebase

All three desktop shells share:
- The same React frontend (`apps/web/`)
- The same Rust wallet backend source (`src-tauri/src/lib.rs`)
- The same pnpm monorepo build system

What differs between them:
- `tauri.conf.json` (window title, identifier, icon paths)
- Platform-specific system prerequisites
- Installer format and distribution channel

If you fix a bug or add a feature in the Rust backend, the change applies to all three platforms when you rebuild.

---

## Environment variables

| Variable | Where | Effect |
|---|---|---|
| `VITE_LIGHTWALLETD_URL` | `apps/web/.env` | Override default lightwalletd in web/PWA builds |
| `ZEC_DESKTOP_BUILD=1` | Set by desktop build scripts | Switches Vite to prerender mode (static HTML, no dev server at runtime) |
