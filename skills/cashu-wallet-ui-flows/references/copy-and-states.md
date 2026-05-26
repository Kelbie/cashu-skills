# Copy And States

Use this reference to choose labels and decide which states belong in primary UI, details, or recovery.

## Primary Action Copy

| Feature | Common labels | Notes |
| --- | --- | --- |
| Send ecash | `Send`, `Send Ecash`, `Create Token`, `Generate Token`, `Create a Cashu token` | Use `token` when the output object is shown or shared. Use `ecash` when the user thinks about value. |
| Receive ecash | `Receive`, `Receive Ecash`, `Redeem Ecash`, `Claim token`, `Paste token` | Use `redeem` or `claim` only when the token will actually be claimed. |
| Payment request create | `Request`, `Payment request`, `Cashu Payment request`, `Create request` | Qualify with Cashu when Lightning invoices or HTTP requests are nearby. |
| Payment request pay | `Pay`, `Confirm`, `Pay via {transport}`, `Pay now` | Show transport if delivery is automatic and user-visible. |
| Lightning receive | `Create invoice`, `Get Invoice`, `Top up`, `Get Ecash`, `Issue Ecash` | Pick `Issue Ecash` only if the minting step is explicit. |
| Lightning pay | `Pay Lightning`, `Pay invoice`, `Pay`, `Cash out` | `Cash out` works when paying outside Cashu, but can obscure Lightning details. |
| Mint transfer | `Transfer`, `Swap`, `Rebalance`, `Receive to trusted mint` | Match the user's intent, not the protocol operation alone. |
| Mint management | `Add mint`, `Trust mint`, `Block receiving`, `Remove mint` | Avoid implying the app operator controls the mint. |
| NFC/POS | `Contactless`, `NFC Payment`, `Tap to pay`, `Scan or tap to pay` | Say what the payload does if the screen is not obvious. |
| Onchain | `Onchain Payment`, `Bitcoin address`, `Network Fee` | Keep onchain separate from Lightning. |

## Copy By Object

| Object | User-facing copy | Detail/recovery copy |
| --- | --- | --- |
| Cashu value | `ecash` | `proofs`, `keysets`, `states` |
| Serialized Cashu token | `Cashu token`, `ecash token`, `token` | encoded token string, token payload |
| Cashu payment request | `Cashu payment request`, `payment request` | NUT-18 request, transport, payment ID |
| Lightning invoice | `Lightning invoice`, `invoice` | BOLT11, mint quote/melt quote |
| LNURL/Lightning address | `Lightning address`, `LNURL` | resolved invoice, min/max sendable |
| Onchain address | `Bitcoin address`, `onchain address` | BIP21/BIP321 URI, confirmations |
| Nostr transport | `Nostr DM`, `Send via Nostr` | NIP-17, nprofile, relays |
| NWC | `Nostr Wallet Connect` | connection string, permissions, daily limits |

## Status And State Ownership

State labels should identify what owns the state.

| Owner | States/copy | UI placement |
| --- | --- | --- |
| Token/proofs | unspent, pending, spent, invalid, already claimed | Receive preview, history detail, recovery. |
| Send operation | prepared, token ready, finalized, rolled back, reclaimed | Send detail/timeline. |
| Mint quote | unpaid, paid, issued, invoice expired | Lightning/onchain receive detail. |
| Melt quote | unpaid, pending, paid, failed | Lightning/onchain pay detail. |
| External rail | invoice paid, payment pending, mempool seen, confirmations | Timeline and rail-specific status. |
| Transport | Nostr sent, HTTP posted, NFC written, delivery failed | Payment request detail or modal. |
| UI loading | Creating, Sending, Redeeming, Checking, Cancelling | Button labels and spinners. |

Avoid saying only `pending`. Prefer `Payment pending`, `Token is pending`, `Invoice still pending`, `Mint quote pending`, or `NFC payment in progress`.

## Success Copy

Choose success copy by completed step:

- Token created but not claimed: `Token created`, `New Cashu token`, `Copy token`.
- Recipient claimed token: `Token has been spent`, `Redeemed`, `Payment successful`.
- Token received into wallet: `Received`, `Claimed`, `Amount received`.
- Lightning invoice paid and ecash issued: `Received via Lightning`, `Ecash created`, `Payment successful`.
- Lightning melt completed: `Paid invoice`, `Payment completed`, `Paid {amount} via Lightning`.
- Transport delivered: `sent!`, `NFC payment sent successfully`, `Payment request has been sent`.

Do not call a mint quote `paid` success if ecash has not been issued yet, unless the screen clearly says the invoice is paid and issuance is next.

## Error Copy

Good error copy tells the user what failed and what to do next.

| Situation | Better copy |
| --- | --- |
| No spendable mint | `No mints with balance available` or `Select a mint to send from`. |
| Unsupported scan | `Bitcoin address payments are not supported` or `Cashu payment requests are not supported from QR yet`. |
| Unknown mint token | `Trust this mint?` plus add/swap/cancel options. |
| P2PK mismatch | `Unable to receive. This token's P2PK lock doesn't match your public key.` |
| Lightning route fail | `Lightning payment failed` plus mint/route detail if available. |
| Quote pending | `Payment pending. Check payment state later.` |
| Recovery risk | `Existing ecash will be deleted and replaced by backup` if true. |

## Detail Sections

Use details sections to expose protocol and audit data without overloading the main action.

Good detail rows:

- `Date`
- `Amount`
- `State`
- `Mint`
- `Source`
- `Format`
- `Payment Methods`
- `Transport`
- `Allowed Mints`
- `Quote ID`
- `Operation ID`
- `Token`
- `Invoice`
- `Address`
- `Network Fee`
- `Memo`
- `Payment hash`

## Timeline Rules

Use timelines for ordered state, not static facts.

Recommended timeline steps:

- Created/prepared.
- Waiting for payer or waiting for receiver.
- External payment observed.
- Confirmations, if onchain.
- Issuing/minting.
- Sent/delivered.
- Finalized.
- Failed, rolled back, reclaimed, or recovered.

If onchain confirmation progress exists, keep it in the timeline unless the product intentionally uses a separate status surface. Duplicate confirmation cards usually create confusion.

## Trust And Custody Copy

Cashu wallets must avoid implying app-operator custody. Good copy patterns:

- `Mints are servers that help you send and receive ecash.`
- `The ecash stored in the wallet is issued by the mint.`
- `This wallet stores ecash that only you have access to.`
- `Before using this mint, make sure you trust it. Mints could become malicious or cease operation at any time.`
- `Anyone with the token can claim it.`

Avoid:

- Saying the app holds funds when the wallet stores bearer ecash locally.
- Saying all mints are equally trusted.
- Saying `your account balance` when the balance is a sum of local proofs issued by several mints.
