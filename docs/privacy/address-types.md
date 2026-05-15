# Address Types

ZecVault supports all Zcash address formats. Understanding what each type exposes — and when to use it — is key to managing your financial privacy.

---

## Overview

| Address | Prefix | Privacy | Use case |
|---|---|---|---|
| Unified (Orchard + Sapling) | `u1...` | Shielded | Default — use this for everything |
| Orchard-only unified | `u1...` | Shielded | Maximum forward privacy |
| Sapling | `zs1...` | Shielded | Compatibility with older wallets |
| Transparent | `t1...` | **Public** | Exchanges, legacy systems only |

---

## Unified Addresses (ZIP-316)

A Unified Address is a single address string that bundles multiple receiver types. When a sender's wallet pays a Unified Address, it automatically picks the best pool it supports:

- If the sender supports Orchard → funds land in your Orchard pool
- If the sender only supports Sapling → funds land in your Sapling pool
- If the sender only supports transparent → funds land in your transparent address (if one is included in the Unified Address)

ZecVault's default Unified Address includes **Orchard + Sapling receivers** — no transparent receiver. This means you always receive into a shielded pool, even from wallets that haven't upgraded to Orchard yet.

---

## Orchard-only Unified Address

An Orchard-only Unified Address contains only an Orchard receiver. Senders without Orchard support cannot pay it — they'll get an error.

Use this when:
- You want to ensure funds always land in Orchard (highest privacy)
- You know the sender's wallet supports Orchard
- You don't want a Sapling fallback

Available in **Expert address mode** (Settings → Advanced).

---

## Sapling address (`zs1...`)

Sapling addresses are fully shielded but use an older format. They're supported by virtually all Zcash wallets and exchanges that support shielded transactions.

Use this when:
- You're interacting with a wallet or service that specifically requires a `zs1` address
- The sender's software doesn't support Unified Addresses

!!! note
    Sapling addresses still offer strong on-chain privacy — amounts and participants are hidden. The difference from Orchard is the proving system, not the privacy level.

---

## Transparent address (`t1...`)

Transparent addresses are fully public — identical in privacy properties to Bitcoin addresses. All transactions to/from a transparent address are visible on blockchain explorers.

Use transparent addresses only when:
- An exchange or service requires it (many centralized exchanges only send to transparent addresses)
- You're interacting with a legacy system that doesn't support shielded addresses

!!! danger "Always shield after receiving"
    If you receive ZEC to a transparent address, shield it immediately using **Send → Shield funds** in ZecVault. Once shielded to Orchard, the funds are private and indistinguishable from other shielded ZEC.

---

## Expert address mode

By default, ZecVault shows only your Unified Address on the Receive screen. Enable **Expert address mode** in **Settings → Advanced** to display all address types and to access advanced send options (choose which pool to spend from, shield to Sapling instead of Orchard, etc.).

---

## One wallet, all addresses

All address types — Unified, Orchard, Sapling, transparent — derive from the same seed phrase and receive into the same wallet account. You don't need a separate wallet for each type. ZecVault tracks balances separately per pool and displays them on the dashboard.
