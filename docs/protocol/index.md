# Protocol

ZecVault is built on the official Zcash protocol stack. This section documents the technical internals — the vault memo protocol, the ZIP standards implemented, and the system architecture for contributors and integrators.

---

<div class="grid cards" markdown>

-   :material-code-tags: **Vault Protocol (ZV1)**

    ---

    The on-chain memo format that links Zcash transactions to vault goals, enabling cross-device deposit reconciliation.

    [:octicons-arrow-right-24: ZV1 memo spec](vault-protocol.md)

-   :material-file-document-check: **ZIP Standards**

    ---

    Which Zcash Improvement Proposals ZecVault implements: ZIP-32, ZIP-302, ZIP-316, ZIP-317, ZIP-321.

    [:octicons-arrow-right-24: ZIP support](zip-support.md)

-   :material-sitemap: **System Architecture**

    ---

    How the React frontend, Rust wallet backend, and lightwalletd fit together. IPC bridge, sync flow, state management.

    [:octicons-arrow-right-24: Architecture](architecture.md)

</div>

---

## Technology stack

### Frontend

| Technology | Role |
|---|---|
| React 19 | UI framework |
| TanStack Router | File-based routing, type-safe navigation |
| Zustand 5 | State management (3 stores, localStorage persistence) |
| Tailwind CSS 4 | Styling with custom design tokens |
| Radix UI | Accessible primitive components |
| Tauri API 2 | IPC bridge to Rust backend |
| Vite 7 | Build tool and dev server |

### Backend (Rust)

| Crate | Role |
|---|---|
| `tauri` 2.10 | Desktop shell + IPC framework |
| `zcash_client_backend` 0.22 | Zcash wallet primitives |
| `zcash_client_sqlite` 0.20 | SQLite-backed wallet database |
| `zcash_keys` 0.13 | Key derivation and management |
| `aes-gcm` 0.10 | AES-256-GCM encryption |
| `argon2` 0.5 | Argon2id key derivation |
| `tonic` 0.14 | gRPC client for lightwalletd |
| `tokio` 1 | Async runtime |
| `keyring` 3 | OS credential store integration |
