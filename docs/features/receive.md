# Receive ZEC

ZecVault generates a Unified Address for your wallet — one address that supports both Orchard and Sapling receivers. Share it and ZEC arrives shielded, no matter what wallet the sender uses.

---

## Your receive address

Open the **Receive** screen from the sidebar. You'll see:

- Your **Unified Address** (`u1...`) — the recommended address for all receives
- A **QR code** for easy scanning
- Your **4-word alias** — a human-readable fingerprint for verification

---

## The 4-word alias

ZecVault displays a **4-word alias** derived from your address (e.g., `planet orbit canvas river`). This is a human-readable fingerprint you can read out loud or verify at a glance before sharing your address.

To verify: compare the 4 words shown on your device with what the sender sees in their wallet. If the words match, the address is correct — no character-by-character comparison needed.

!!! warning "Always verify before large transfers"
    The 4-word alias is a convenience verification tool, not a replacement for address confirmation. For large amounts, verify both the alias and the first/last characters of the full address.

---

## Address types

By default, the Receive screen shows your **Unified Address** with Orchard + Sapling receivers. In **Expert address mode** (Settings → Advanced → Expert address mode), you can display additional address types:

| Type | Starts with | Privacy | When to use |
|---|---|---|---|
| Unified (Orchard + Sapling) | `u1` | Shielded | Default — use this whenever possible |
| Orchard-only unified | `u1` | Shielded | Maximum forward privacy; some older senders can't pay it |
| Sapling | `zs1` | Shielded | For wallets that don't support Orchard yet |
| Transparent | `t1` | **Public** | Exchanges, legacy integrations — avoid for routine use |

All address types receive into the same wallet account. The sender's wallet automatically picks the best pool it supports when using a Unified Address.

---

## Receiving into a vault

To receive ZEC directly into a vault:

1. Open the vault in the **Vaults** screen
2. Tap **Deposit on-chain**
3. Share the vault's dedicated receive address with the sender

When the sender includes a `ZV1:<vaultId>:<goalName>` memo, the app links the incoming transaction to that vault automatically. You can also ask a sender to pay your main Unified Address and manually log the deposit later.

---

## QR codes

The QR code on the Receive screen encodes your Unified Address. Most Zcash wallets can scan it directly from the Send screen camera.

For vaults or payment requests with a specific amount, use a **payment URI** instead — the QR encodes the full `zcash:` URI including address, amount, and memo.

---

## Privacy when receiving

Funds sent to your shielded (Unified or Sapling) address are private:

- The amount is hidden from on-chain observers
- The sender and recipient are cryptographically concealed
- Only you (and anyone with your viewing key) can see the transaction details

If someone sends to your **transparent** address, the transaction is public on-chain. ZecVault will notify you and offer to shield those funds to your Orchard balance.

---

## Sharing your address safely

Your Unified Address is safe to share publicly — it's the equivalent of sharing your bank account number for receiving payments. No private information is revealed by your address alone.

The only risk of sharing your address is **linking your identity to an address**. If you share the same address with multiple parties, those parties could potentially correlate your activity. For maximum privacy, ZecVault generates a fresh address per vault — consider using vault addresses for different recipients.
