# Savings Vaults

Vaults are ZecVault's signature feature. A vault is a named savings goal — you give it a purpose, set a ZEC target and a deadline, and deposit toward it over time. The app tracks your progress, shows a contribution streak, and makes you wait 24 hours before breaking it early.

---

## The core idea

Vaults don't lock your ZEC on-chain. Your funds always remain in your Zcash wallet — the vault is an **accounting layer** on top of your regular balance.

When you deposit into a vault, the app:

1. Records the deposit in the vault's contribution list
2. Subtracts that amount from your **displayed spendable balance** — you won't accidentally spend it
3. Optionally broadcasts a Zcash transaction to the vault's receive address with an encrypted labelling memo

The soft-lock is enforced by the app, not a smart contract. The 24-hour cooldown before breaking a vault is by design — it's a tool for **self-accountability**, not cryptographic enforcement.

---

## Creating a vault

Navigate to the **Vaults** screen and tap **New vault**. You'll set:

| Field | Description |
|---|---|
| Name | What you're saving for (e.g., "Bali Trip 2026") |
| Category | Visual theme — see categories below |
| Target amount | ZEC goal (e.g., 5 ZEC) |
| Deadline | Optional date target |

### Categories

| Category | Label |
|---|---|
| `trip` | Experiences |
| `ring` | Wedding |
| `house` | Home Upgrade |
| `car` | EV Fund |
| `emer` | Safety Buffer |
| `gift` | Family Moments |
| `edu` | Skills & Courses |
| `tech` | Creator + AI Gear |
| `custom` | (your own label) |

Categories are cosmetic. They change the icon and color of the vault card — not its behavior.

---

## Depositing into a vault

There are two ways to deposit:

### Manual deposit (local-only)

Tap **Add deposit** on a vault card and enter an amount. The app records it locally and adjusts your displayed balance immediately — no Zcash transaction is created.

Use this for round-up bookkeeping or when you want to batch deposits.

### On-chain deposit (recommended)

Send ZEC to the vault's dedicated receive address with a ZV1 memo included. The memo is embedded inside the shielded transaction:

```
ZV1:<vaultId>:<goalName>
```

This creates an on-chain, encrypted record that survives device restores. After syncing on any device, the app scans incoming transactions for ZV1 memos and automatically credits the matching vault.

---

## Vault lifecycle

```
active  →  breaking  →  archived   (early exit with 24h wait)
active  →  complete  →  archived   (goal reached)
```

| State | What it means |
|---|---|
| **active** | Accepting deposits, progress growing |
| **breaking** | You've initiated an early exit; 24-hour countdown running |
| **complete** | Target reached; vault moves to archive |
| **archived** | Closed and read-only (completed or broken) |

You can cancel a break request at any time before the 24 hours expire — the vault returns to **active** status.

---

## Balance accounting

The app maintains a virtual spendable balance:

> **Displayed spendable = total chain balance − sum of all active vault balances**

Vault funds don't appear in your sendable balance, preventing accidental spending. When a vault completes or is broken, the amount returns to your displayed spendable balance.

---

## Streak tracking

The vault card shows a **streak counter**: the number of consecutive calendar days you've made at least one deposit. The streak:

- Increments the day after your last deposit
- Resets if a full day passes without any deposit
- Is tracked locally per device — not tied to on-chain data

Streaks are a motivation tool. Building a daily savings habit is the point.

---

## What happens after restore?

Vault goals are stored in `localStorage` on each device. After restoring from seed on a new device:

1. The **on-chain balance** is fully recovered after sync
2. **ZV1 memo deposits** are re-linked to vaults automatically via transaction history reconciliation
3. **Local-only deposits** (manual bookkeeping entries) are not recoverable — they have no on-chain record

For this reason, on-chain vault deposits (with ZV1 memos) are recommended for any deposit you'd want to survive a device change.

---

!!! tip "Privacy of vault memos"
    ZV1 memos are encrypted inside shielded Zcash notes. Nobody observing the blockchain can read them — only you (with your viewing key) can see that this transaction was a vault deposit and which vault it went to.
