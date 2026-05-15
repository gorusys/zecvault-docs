# Lightwalletd & Metadata Privacy

ZecVault syncs with the Zcash network using **lightwalletd** — a server-side indexer that serves compact blocks over an encrypted gRPC/TLS connection. This section explains what the server can and cannot see, and how to minimize metadata exposure.

---

## What is lightwalletd?

A full Zcash node stores 50+ GB of block history and scans the entire chain locally. Lightwalletd is a middle-ground: it downloads and indexes the chain, then serves **compact blocks** to light wallets. Compact blocks are small summaries — enough data for your wallet to detect incoming notes, without the full block data.

ZecVault downloads these compact blocks and scans them locally using your private keys. The lightwalletd server never sees your keys.

---

## What the server can see

| Information | Visible to lightwalletd? | Notes |
|---|---|---|
| Your IP address | **Yes** | Inherent to any TCP connection |
| Which blocks you download | **Yes** | Server sees your request range |
| When your wallet syncs | **Yes** | Derived from download timing |
| Transactions you broadcast | **Yes** | You submit broadcasts to the server |
| Contents of shielded transactions | **No** | Encrypted; only you can decrypt |
| Your addresses | **No** | Never sent to the server |
| Your balance | **No** | Computed locally |
| Your memo content | **No** | Encrypted inside shielded notes |

The server knows you're using a Zcash wallet and roughly when you're active — but it cannot see what you're doing.

---

## The default server

ZecVault defaults to **`zec.rocks:443`** — a community-operated lightwalletd instance with a strong privacy policy and high uptime. You don't need an account and no identifying information is collected.

---

## Running your own lightwalletd

For maximum metadata privacy, run your own lightwalletd instance. When you control the server, no third party sees your sync requests.

### Quick setup with Docker

```bash
docker pull electriccoinco/lightwalletd
docker run -p 9067:9067 \
  -v /path/to/zcash.conf:/root/.zcash/zcash.conf \
  electriccoinco/lightwalletd \
  --grpc-bind-addr 0.0.0.0:9067 \
  --zcash-conf-path /root/.zcash/zcash.conf
```

Then point ZecVault to your instance:

**Settings → Network → Lightwalletd server → `your-server:9067`**

---

## Pointing ZecVault to a custom server

1. Open **Settings → Network**
2. Tap the **Lightwalletd server** field
3. Enter your server URL (e.g., `https://my-lightwalletd.example.com:443`)
4. Tap **Save** — the wallet will reconnect and validate the endpoint

The server must support gRPC over TLS. Self-signed certificates are accepted.

---

## Additional metadata protection

| Technique | How |
|---|---|
| **Tor** | Route ZecVault's network traffic through Tor to hide your IP from the lightwalletd server |
| **VPN** | A VPN hides your IP but the VPN provider can see your connection |
| **Self-hosted lightwalletd** | Eliminates the third-party server entirely |
| **Birthday height** | Setting an accurate birthday height reduces the block range the server sees you download |

!!! info "Tor on desktop"
    ZecVault doesn't have built-in Tor support yet. To use Tor, configure your OS or a SOCKS5 proxy and route traffic system-wide, or run `torsocks` before launching ZecVault.
