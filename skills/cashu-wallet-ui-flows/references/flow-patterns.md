# Flow Patterns

This reference gives verbose, product-neutral descriptions of common Cashu wallet UI flows. Each feature includes a flow spine, observed variants, copy options, and state decisions.

## Home, Hub, And Navigation

Cashu wallets commonly start with a balance surface and recent activity. The main divergence is how the wallet exposes rails.

Action-first home:

1. Show aggregate balance and unit.
2. Show recent activity, often the latest 3 to 10 entries.
3. Show primary actions: `Send`, `Scan`, `Receive`.
4. `Send` opens a sheet or menu with `Send Ecash`, `Pay Lightning`, and sometimes `NFC Payment`.
5. `Receive` opens `Paste token` or `Receive Ecash`, `Create Lightning invoice`, and sometimes `Payment request`.

Rail menu home:

1. Show balance and history.
2. `Receive` opens rail choices: `Request`, `Ecash`, `Lightning`.
3. `Send` opens rail choices: `Ecash`, `Lightning`, `Contactless`.
4. Global scan can parse token, invoice, payment request, LNURL, Lightning address, or merchant code and route directly.

Tabbed web-style home:

1. Show Send and Receive actions.
2. Show tabs for `History`, `Invoices`, and `Mints`.
3. Dialogs handle token send, token receive, invoice creation, and invoice payment.
4. Advanced actions live in settings or mint details.

Implementation guidance:

- Use action-first when the user thinks "send/receive" before they know the rail.
- Use rail-first when the wallet has many rails and users are expected to understand them.
- Keep global scan useful only if it handles multiple formats and errors clearly.

## Sending Ecash

Flow spine:

1. Enter or select amount.
2. Select mint/proofs if multiple sources can pay.
3. Optionally add memo, recipient, transport, or P2PK lock.
4. Prepare send operation.
5. Confirm if the operation reserves proofs or changes wallet state.
6. Display or deliver the token.
7. Track redeemed/spent status and offer recovery/cancel where possible.

Variant: amount-first with confirmation

- Screen title: `Send Ecash` or `Select Amount`.
- Header or selector shows the active mint.
- Amount input validates minimum amount and balance.
- Continue prepares an operation and opens a confirmation modal.
- Confirm executes and navigates to a token screen.
- Token screen shows QR, amount, mint, copy/share actions, and sometimes a spent state.

This is best when the wallet wants a narrow, modern payment flow and strong cancellation semantics.

Variant: form-first token generation

- Screen title: `Send`.
- Form includes amount, `Send from` mint selector, balance row, optional memo, and optional `Lock to Public Key`.
- Primary button says `Generate Token`.
- After success the same screen replaces form-only state with token sharing controls.

This is best when advanced token parameters are part of the user's mental model.

Variant: send with transport choices

- User enters amount and mint.
- Token can be copied, shared, sent via Nostr DM, sent via HTTP POST, or transmitted through NFC.
- Contacts may prefill recipient, relay hints, and transport.
- Transport progress uses states like sending, success, failure, and fallback token display.

This is best for social/contact wallets and payment-request implementations.

Copy patterns:

- Entry: `Send`, `Send Ecash`, `Create Token`, `Create a Cashu token and send it to anyone`.
- Object handoff: `Copy token`, `Share`, `Copy as sendable tokens`, `Copy as emoji`, `NFC`.
- State actions: `Check Status`, `Cancel Transaction`, `Reclaim proofs`.
- Warnings: `Not enough funds`, `Balance too low`, `No mints with balance available`.

State decisions:

- `prepared`: proofs are reserved but token may not be final.
- `finalized`: token is created and shown/delivered.
- `spent` or `redeemed`: recipient claimed it; hide or mark token as spent.
- `rolled_back` or `reclaimed`: sender recovered proofs; token should not be presented as payable.

## Receiving Or Redeeming Ecash

Flow spine:

1. User pastes, scans, taps NFC, opens a link, or auto-claims clipboard.
2. Wallet decodes the token and extracts amount, unit, mint, memo, and lock status.
3. Wallet evaluates mint trust and token spendability when online.
4. User chooses receive action: redeem, trust/add mint, swap to trusted mint, receive later, or cancel.
5. Wallet claims token and records history.
6. Success view shows amount, mint, memo, fee if any, and status.

Variant: receive hub with paste/fixed amount/scan

- Receive home shows a Lightning address or P2PK key, plus buttons like `Paste`, `Fixed Amount`, and `Scan QR`.
- Paste/scan routes to token preview if a Cashu token is found.
- Fixed amount routes to a receive amount flow or payment request flow.

Variant: token preview then redeem

- Screen title: `Receive Ecash`.
- Preview shows token text or QR-derived data, amount, memo, mint, and lock state.
- Primary button says `Redeem Ecash`, `Receive`, or `Claim token`.
- If already redeemed/spent, show `Invalid token` or `Token invalid or already claimed`.

Variant: unknown mint decision

- Trust/add path: `Unknown mint. It will be added after you receive this token.`
- Swap path: show source mint, destination mint, and fee warning; copy can be `Receive to trusted mint` or `Swap to a trusted mint`.
- Receive later path: add token to history without claiming, usually for locked tokens or unavailable mints.

Variant: auto-claim from clipboard

- Receive sheet offers `Paste token`.
- Wallet reads clipboard, attempts claim, and shows a prompt/toast on success or invalid clipboard.
- This is fast but can feel opaque; keep error copy precise.

Copy patterns:

- Entry: `Receive`, `Receive Ecash`, `Paste ecash token`, `Scan or paste to receive`.
- Action: `Redeem Ecash`, `Receive`, `Claim token`, `Receive offline`.
- Unknown mint: `Trust this mint?`, `Trust mint`, `Receive to selected mint`, `Receive later`.
- Lock: `Locked to your keys`, `Can not unlock`, `Lock to public key`.

## Cashu Payment Request Creation

Flow spine:

1. Enter amount or leave amount optional.
2. Choose allowed mints or accept any mint.
3. Choose transport: Nostr, HTTP POST, NFC, or manual/in-band.
4. Optionally lock response token to wallet key.
5. Generate encoded request.
6. Show QR, copy/share, payment ID, transport, and allowed mint details.
7. Wait for payment or return to history.

Observed variants:

- A request item inside `Receive` labeled `Request` or `Create payment request`.
- A dedicated `Payment Request` screen with amount, `Request from`, `Receive via`, and `Lock to wallet key`.
- A mint selector sheet titled `Mint to receive` with confirmation `Create request`.
- Request QR screen titled `Cashu Payment request`.

Copy patterns:

- `Payment request`, `Cashu Payment request`, `Create payment request`, `Create request`, `Request from`, `Receive via`, `Send payment request`.
- Transport detail: `Nostr DM`, `HTTP`, `Payment request via {transport}`.
- Mint detail: `Any mint`, selected mint count, `Receive to`.

Implementation guidance:

- If Lightning invoices are nearby, qualify as `Cashu payment request`.
- If only Cashu request UI exists on the screen, `Payment request` is enough.
- Show transport early if the payer must be able to return the token automatically.

## Paying A Cashu Payment Request

Flow spine:

1. Parse encoded request from scan, paste, deep link, NFC, or contact flow.
2. Resolve amount; if absent, ask payer to enter one.
3. Resolve allowed mints and select one with enough balance.
4. Show recipient/transport and amount.
5. Prepare token.
6. Deliver token via Nostr, HTTP POST, NFC write-back, or manual display.
7. Record send history and show completion or fallback.

Observed variants:

- Payment request preview screen with `Confirm` and `Cancel`.
- Mint selector titled `Pay from` with `Pay now`.
- Transport selector `Send via` with Nostr relay connection count or HTTP endpoint.
- Fallback token display when transport cannot be reached.

Copy patterns:

- `Pay`, `Pay via {transport}`, `Pay to {target}`, `Confirm`, `Sending...`.
- Error: `None of the mints accepted by this payment request are in your wallet.`
- Error: `Not enough balance to pay this payment request.`

## Lightning Receive, Top Up, Or Issue Ecash

Flow spine:

1. Enter amount.
2. Select receive mint.
3. Create mint quote.
4. Display Lightning invoice as QR/text/share.
5. Wait for external payment.
6. Issue ecash after quote is paid.
7. Show success/history with mint and amount.

Variant: invoice-first details screen

- Title: `Receive Lightning` or `Create Invoice`.
- Amount input uses `Amount ({ticker})`.
- Primary button: `Create Invoice`, then invoice screen shows `Lightning invoice`, copy/share, and `Paid!`.

Variant: issue-oriented native screen

- Title: `Issue Ecash`.
- Amount and `Mint` selector appear first.
- Button starts as `Get Invoice`, then becomes `Issue Ecash` or `Issue`.
- Polling or manual check detects invoice paid, then issue adds proofs and closes.
- Details may expose quote ID and expiry.

Variant: receive address

- Receive screen shows a Lightning address as a QR/copyable value.
- Incoming payments are auto-claimed or manually checked.
- Settings decide whether background checks or auto-claim are enabled.

Copy patterns:

- `Top up wallet`, `Get Ecash`, `Create invoice`, `Create Lightning invoice`, `Get Invoice`, `Issue Ecash`, `Share invoice`.
- State: `Awaiting payment`, `Invoice still pending`, `Invoice expired`, `Received via Lightning`, `Mint quote paid`, `Issue`.

## Lightning Pay Or Melt

Flow spine:

1. Enter, paste, scan, or receive a Lightning invoice, LNURL, or Lightning address.
2. Resolve LNURL/Lightning address. If variable amount, ask for amount.
3. Select mint or mints.
4. Create melt quote and estimate fee.
5. Confirm payment.
6. Execute melt.
7. Show success, pending, in-progress, or error.

Variant: target-first pay dialog

- Title: `Pay Lightning`.
- Input label: `Lightning invoice or address`.
- Actions: `Paste`, `Scan`, `Enter`.
- Invoice preview shows amount, memo, fee, balance warning, and `Pay`.

Variant: full-screen amount/fee flow

- Header shows selected mint and amount.
- Card shows memo, LNURL/domain, invoice description, and optional comment.
- Mint selector sheet says `Pay from` and `Pay now`.
- Result modal says `Payment completed`, `Payment failed`, `Payment is pending`, or `Payment in progress`.

Variant: multi-mint pay

- Wallet selects one mint with enough balance, or multiple MPP-capable mints if no single mint can cover the invoice.
- UI shows `Pay from N mints`, total Lightning fees, and per-mint quote states.
- Error states can be per mint.

Copy patterns:

- Entry: `Pay Lightning`, `Pay invoice`, `Pay a Lightning invoice`, `Invoice or Address`, `LN invoice or LNURL`.
- Fee: `Fee`, `Estimated fees`, `Total incl. fee`, `Invoice incl. fee`.
- Pending: `Check Payment State`, `Payment Pending`, `Payment in progress`.
- Error: `Insufficient balance (including fees)`, `Lightning payment failed`.

## Mint Transfer, Swap, And Rebalance

Flow spine:

1. Choose source mint and target mint.
2. Enter amount.
3. Create target mint invoice and source melt quote.
4. Pay from source mint.
5. Issue on target mint.
6. Show fee, route, and result.

Variants:

- `Transfer between mints`: explicit source and target.
- `Receive to trusted mint`: incoming token from unknown/untrusted mint is swapped before becoming trusted balance.
- `Rebalance`: suggested distribution or path-based movement.

Copy patterns:

- `Transfer`, `Swap`, `Multimint swap`, `Receive to trusted mint`, `Transfer to`, `From`, `To`.
- Warnings should mention Lightning fees and possible incomplete receive/issue states.

## Onchain Receive And Send

Receive flow spine:

1. Enter amount.
2. Select mint.
3. Create onchain mint quote.
4. Show onchain address or `bitcoin:` URI as QR/copy/share.
5. Track mempool and confirmations.
6. Refresh quote and issue ecash once required confirmations are satisfied.
7. Show address, network fee copy, quote ID, mint, and timeline.

Send flow spine:

1. Parse Bitcoin address/URI or enter address.
2. Enter amount and fee options if supported.
3. Select mint/source balance.
4. Quote, confirm, and execute.
5. Track broadcast/settlement state.

Copy patterns:

- `Onchain Payment`, `Bitcoin address`, `onchain address`, `Network Fee`, `Paid by sender`.
- Unsupported path: `Bitcoin address payments are not supported`.

## NFC And POS

Paying via NFC:

1. User chooses `NFC Payment` or `Contactless`.
2. Screen checks NFC support and enabled state.
3. User holds device near terminal.
4. Wallet reads payment request or invoice.
5. Wallet prepares ecash token or Lightning payment.
6. Wallet writes token back over NFC or uses request transport.
7. Success says payment sent or failed with reason.

Receiving/POS:

1. Receiver enters amount.
2. Wallet chooses receive mint.
3. Wallet creates invoice or payment request.
4. Screen shows QR and/or waits for NFC payer.
5. Status says `Waiting for payment...`, then `Payment completed`.

Copy patterns:

- `NFC Payment`, `Contactless`, `Tap to pay at a terminal`, `Scan or tap to pay`, `Waiting for payment...`.
- Error: `NFC is not supported`, `Please enable NFC`, `NFC Payment failed`.

## Mint Management

Flow spine:

1. List mints with name/icon, URL/host, unit, balance, and status.
2. Add mint by URL, search, recommendation, or token receive.
3. Show mint details and warnings before trust.
4. Allow management actions: rename, copy URL, share URL, block/unblock receiving, delete/remove, refresh settings.
5. Show advanced info: NUTs, limits, contact, MOTD, keysets, proof checks.

Copy patterns:

- `Add mint`, `Add mint URL`, `Manage mints`, `Mint info`, `Mint URL`, `Supported NUTs`, `Limits`, `Important notice`.
- Trust: `Before using this mint, make sure you trust it. Mints could become malicious or cease operation at any time.`
- Delete: `Remove this mint?`, `Unable to remove a mint with remaining balance`.

## History And Recovery

History flow:

1. List recent entries on home, with full activity in a separate screen.
2. Entry details show amount, type, status, mint, date, memo, fee, source, and rail identifiers.
3. Timeline shows local and external rail progress.
4. Actions depend on state: refresh, copy token, check spent, reclaim/cancel, retry.

Recovery flow:

1. User chooses seed recovery, backup import, quote recovery, or proof cleanup.
2. Wallet explains preconditions: battery, Wi-Fi, foreground, mint URLs, nonzero-balance overwrite risk.
3. User selects mint(s) and starts restore.
4. Progress shows found proofs, pending proofs, spent proofs, recovered amount, or no ecash.

Copy patterns:

- `Restore ecash`, `Recover mint quote`, `Recover melt quote change`, `Seed backup`, `Wallet backup`, `Export ecash`, `Check proofs`.
- Recovery can expose `proofs`, `keysets`, `mint quote`, and `melt quote` because the user is fixing technical state.
