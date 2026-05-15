# Round-Up Savings

Round-up savings is an opt-in feature that automatically captures the "spare change" from every ZEC you send and deposits it into a vault. It's a lightweight habit — save without thinking about it.

---

## How it works

After every outgoing transaction, the app calculates the difference between your send amount and the nearest round-up threshold. That difference is credited as a manual deposit to your chosen vault.

### Example

| Setting | Send amount | Nearest threshold | Round-up captured |
|---|---|---|---|
| 0.01 ZEC threshold | 0.342 ZEC | 0.35 ZEC | 0.008 ZEC |
| 0.1 ZEC threshold | 0.342 ZEC | 0.4 ZEC | 0.058 ZEC |
| 1 ZEC threshold | 0.342 ZEC | 1.0 ZEC | 0.658 ZEC |

---

## Threshold options

| Threshold | Captured per send | Best for |
|---|---|---|
| **0.01 ZEC** | Small amounts (sub-cent) | Frequent micro-savings habit |
| **0.1 ZEC** | Moderate amounts | Balanced savings without large deductions |
| **1 ZEC** | Larger amounts | Fast goal accumulation |

---

## Enabling round-up savings

1. Go to **Settings → Round-up savings**
2. Toggle it **On**
3. Select your threshold (0.01, 0.1, or 1 ZEC)
4. Choose which vault receives the round-up

You can change the target vault or threshold at any time.

---

## Important notes

- **Round-ups are local deposits only** — no extra Zcash transaction is created. The app credits your vault balance without a network fee.
- **The captured amount is deducted from your displayed spendable balance**, just like any other vault deposit.
- **Round-ups don't appear as separate on-chain transactions** — if you restore from seed, they won't be reconciled automatically. Only ZV1 memo-linked deposits survive restore.

!!! tip "Pair with a streak goal"
    Enable round-up savings alongside a streak-tracked vault to build a daily savings habit effortlessly. Every send you make contributes to your goal automatically.
