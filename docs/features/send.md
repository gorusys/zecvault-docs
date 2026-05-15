# Send ZEC

ZecVault uses a two-step send process: first it previews the exact fee, then you confirm before broadcasting. This ensures you always know the cost before committing.

---

## How to send

### 1. Open Send

Tap **Send** in the sidebar navigation.

### 2. Enter the recipient address

Paste or scan a Zcash address. ZecVault supports:

- **Unified Addresses** (`u1...`) — recommended; Orchard + Sapling compatible
- **Sapling addresses** (`zs1...`) — shielded, older format
- **Transparent addresses** (`t1...`) — public, like Bitcoin; use only when required
- **Payment URIs** (`zcash:address?amount=...`) — auto-fill amount and memo from a QR code

!!! tip "Address privacy classification"
    When you enter an address, the app shows a privacy indicator:
    - 🟢 **Shielded** — sending to Orchard or Sapling; amounts are private
    - 🟡 **Transparent** — sending to a transparent address; the amount and recipient are public on-chain

### 3. Enter amount

Type the amount in ZEC. The app shows the equivalent in your selected fiat currency (USD, EUR, GBP, JPY, SGD).

### 4. Add a memo (optional)

For sends to shielded addresses, you can add up to **512 bytes** of text as an encrypted memo. Only the recipient can read it. Memos are not available for transparent sends.

Common uses: invoice references, personal notes, payment descriptions.

### 5. Preview the transaction

Tap **Preview**. ZecVault asks the wallet backend to build the transaction without broadcasting it and returns the **exact fee**. You'll see:

- Recipient address
- Amount being sent
- Network fee (calculated per ZIP-317)
- Total deducted from your balance

### 6. Confirm and send

After reviewing the fee, tap **Send**. The backend signs the transaction with your spending key and broadcasts it to the Zcash network via lightwalletd. Your key is used only during signing and immediately discarded from memory.

---

## Send Max

**Send Max** drains your entire spendable shielded balance (Orchard + Sapling) to a recipient in one transaction. The network fee is automatically deducted — the recipient receives exactly what's left.

To use it: open Send, enter a recipient, and tap **Send Max** instead of typing an amount.

---

## Shielding transparent funds

If you receive ZEC to a transparent address (from an exchange, for example) and your shielded balance isn't enough to cover a send, ZecVault will offer to **shield** your transparent funds first.

Shielding moves transparent funds into your Orchard pool via a self-send. Once shielded, the funds are private and can be sent normally.

You can also trigger shielding manually from the Send screen under **Shield funds**.

---

## Pool spend order

When building a transaction, ZecVault spends from pools in this order:

1. **Orchard** — most private, spent first to preserve Sapling funds
2. **Sapling** — shielded, used if Orchard is insufficient
3. **Transparent** — only included if shielding is needed

This order keeps funds in the most private pool for as long as possible.

---

## Payment URIs

ZecVault supports **ZIP-321 payment URIs** (`zcash:` links). When you scan a QR code containing a payment URI, the app auto-fills the recipient, amount, and memo. You can still edit any field before confirming.

Example URI:
```
zcash:u1exampleaddress...?amount=1.5&memo=SW52b2ljZSAjMTIz
```

(Memo is base64-encoded in URIs.)

---

## Fee calculation

Fees follow **ZIP-317**. The fee scales with the complexity of the transaction (number of inputs and outputs), not a fixed rate. Typical shielded sends cost **0.00001 ZEC** (1,000 zatoshis). Complex transactions with many inputs cost proportionally more.

The fee preview step always shows the exact fee before you commit — no surprises.

---

## Sapling → Orchard migration

If you have funds in the Sapling pool from an older wallet or exchange, you can migrate them to Orchard via **Wallet → Migrate Sapling to Orchard**. This performs a self-send — your ZEC moves from Sapling to Orchard in one transaction. The fee is deducted from the amount being migrated.
