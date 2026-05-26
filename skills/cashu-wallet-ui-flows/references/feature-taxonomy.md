# Feature Taxonomy

Use this as the coverage checklist for a Cashu wallet UI. A wallet does not need every feature, but every supported feature should have a deliberate entry point, sequence, copy style, and state model.

## Home And Action Hub

The wallet home usually answers: current balance, active unit, recent activity, and the next action. Observed layouts split into:

| Variant | Description | Best fit |
| --- | --- | --- |
| Action dock | Persistent Send, Scan, Receive actions near the balance. | Mobile wallets where speed matters. |
| Two-action menu | Send and Receive each open a rail menu. | Wallets with many rails but limited home space. |
| Tabbed wallet | History, invoices, mints, and settings are top-level tabs. | Web or desktop wallets with more room. |
| Flow routes | Each action is a dedicated full-screen route. | Apps with stronger guided flows and auditability. |

Required decisions:

- Is the first choice the action (`Send`, `Receive`) or the rail (`Ecash`, `Lightning`, `Onchain`)?
- Is scan global, or does scan belong inside send/receive?
- Does the home show pending states inline, or only inside history/detail screens?
- Does the balance aggregate all mints, show per-unit tabs, or show per-mint rows?

## Scan And Input

Scan/input is the router between rails. It should classify at least the formats the wallet supports:

- Cashu token: route to receive/redeem token.
- Cashu payment request: route to payment request pay.
- Lightning invoice: route to pay Lightning.
- LNURL or Lightning address: route to LNURL pay or invoice creation.
- Bitcoin URI/address: route to onchain if supported; otherwise show unsupported copy.
- Public key: route to P2PK lock input if supported.
- Unknown input: explain supported formats.

Clean scan copy names supported formats instead of saying only `Scan QR`. Good supporting copy says `Cashu, Lightning, LNURL, or Bitcoin` only when those paths actually work.

## Send Ecash

Sending ecash creates a Cashu token or sends a payment-request payload. The UI should decide:

- Amount-first or recipient/payment-request-first?
- Single mint, selectable mint, proof selection, or multi-mint spend?
- Is the token shown as QR, text, link, emoji, NFC, Nostr DM, HTTP POST, or share sheet?
- Does the wallet track whether the token was redeemed and allow cancel/reclaim?
- Does offline send produce a local pending token, or block sending?
- Does P2PK locking belong in the primary flow or advanced controls?

## Receive Or Redeem Ecash

Receiving ecash claims a Cashu token into the wallet. The UI should decide:

- Paste-first, scan-first, or auto-claim from clipboard?
- Is the decoded token preview shown before redeem?
- What happens when the mint is unknown: reject, trust/add, receive later, or swap to a trusted mint?
- How are P2PK-locked tokens shown: locked to wallet, locked to unknown key, missing key, partial match?
- Is offline receive allowed as a prepared/local state?
- Does success say `Received`, `Redeemed`, or `Claimed`?

## Cashu Payment Requests

Payment requests are ecash requests, not Lightning invoices. Treat them as a two-sided feature.

Create side:

- Amount required, optional, or open amount?
- Any mint accepted, selected mints, or one receive mint?
- Which transport returns the token: Nostr, HTTP POST, in-band/manual, NFC, or QR copy?
- Is the request single-use or reusable?
- Is the request P2PK-locked to the receiving wallet?

Pay side:

- Parse request first, then ask amount if missing.
- Select an allowed mint with enough balance.
- Confirm the amount, mint, recipient/transport, and fee if any.
- Execute token creation and deliver through the chosen transport.
- Provide fallback token display if transport delivery fails.

## Lightning Receive

This is minting ecash by paying a Lightning invoice from outside the wallet.

Required sequence:

1. Select amount.
2. Select receive mint.
3. Create mint quote and Lightning invoice.
4. Show invoice as QR/text/share.
5. Poll, subscribe, or provide manual check.
6. Issue ecash after the invoice is paid.
7. Show history/detail state.

Variant decisions:

- Is the screen titled `Receive Lightning`, `Create invoice`, `Top up`, `Get Ecash`, or `Issue Ecash`?
- Does the wallet issue automatically on payment, or require a user `Issue`/`Check payment` action?
- Does the wallet expose quote ID, expiry, and mint details?
- Does the wallet support Lightning address auto-claim or Nostr zap collection?

## Lightning Pay

This is melting ecash to pay a Lightning invoice, LNURL pay target, or Lightning address.

Required sequence:

1. Input or scan payment target.
2. Resolve LNURL/Lightning address to an invoice if needed.
3. Show or ask amount when the invoice does not fix it.
4. Select a mint or several mints.
5. Estimate fee and total.
6. Confirm payment.
7. Show success, pending, in-progress, or failure with recovery action.

Variant decisions:

- Target-first or amount-first?
- Single mint only, automatic mint selection, manual mint selector, or multi-mint MPP?
- Does the UI show fee before confirmation?
- Does pending mean the external payment is async, the mint quote is pending, or local proofs are reserved?
- Does the UI offer retry, cancel, reclaim, or check state?

## Mint Transfer And Rebalance

Moving value between mints usually melts from one mint to an invoice from another mint, then issues at the target mint.

The UI should show:

- Source mint and target mint.
- Amount and expected fee.
- Whether a route exists.
- Setup, paying, minting, completed, and failed stages.
- Recovery path when melt paid but issue is incomplete.

Primary copy can be `Transfer`, `Swap`, `Rebalance`, or `Receive to trusted mint`. Pick based on user intent:

- `Transfer` when the user chooses source and target mints.
- `Swap` when moving an incoming token away from an untrusted mint.
- `Rebalance` when correcting portfolio distribution.

## Mint Management And Trust

Mint management is both wallet organization and safety UX.

Feature coverage:

- Add mint from URL, search/discovery, recommendation, or token receive.
- Show mint URL, name/icon, version, units, NUT support, limits, contact, description, MOTD.
- Trust/add, block receiving, unblock, rename, set color, delete/remove.
- Show per-mint balance by unit.
- Check proofs, remove spent proofs, clear reserved/pending proofs.
- Restore from mint using seed or quote.
- Show warnings that mints can fail, censor, or become malicious.

## Onchain Bitcoin

Onchain support is not universal. If present, keep it distinct from Lightning:

- Receive onchain: amount -> mint -> quote/address -> QR/share -> mempool/confirmation progress -> issue ecash.
- Send onchain: address/URI -> amount/fee quote -> mint/source selection -> confirm -> broadcast/external settlement state.
- BIP21/BIP321 parsing may show multiple payment methods. The UI should indicate which one is used.
- Confirmation progress belongs in the transaction timeline, not in a separate duplicate status card unless the product intentionally uses both.

If unsupported, the scan/input error should say Bitcoin address payments are not supported rather than silently falling back to Lightning.

## NFC And POS

NFC/POS flows are two-sided:

- Paying at a terminal: read payment request or Lightning invoice over NFC, prepare payment, write token back or deliver through transport, then show success/failure.
- Receiving in person: create payment request or invoice, wait for payer, show QR/NFC readiness.
- Writing a token to an NFC tag/card: created token -> NFC write -> success/error.

Important decisions:

- Does NFC transfer a Cashu token, a Cashu payment request, or a Lightning invoice?
- Is the NFC session continuous through read, prepare, and write?
- What copy appears while the user holds the device near the reader?
- What fallback is shown when NFC is disabled or unsupported?

## History And Details

History is where technical state can become visible. Include:

- Type: mint, receive, send, melt, transfer, swap, onchain.
- Amount, unit, mint, date, state.
- Quote ID, operation ID, token preview, invoice/address, payment hash where relevant.
- Timeline with prepared, pending, paid, issued, finalized, rolled back, failed, reclaimed.
- Refresh/check status where the external rail or mint may update later.
- Location/method labels for BIP21/BIP321 or scan source if useful.

## Backup, Recovery, And Onboarding

The wallet must teach enough custody reality to prevent loss.

Feature coverage:

- Seed backup during onboarding or later.
- Explain that ecash is stored locally and issued by mints.
- Restore by seed, but require selecting/restoring from known mints.
- Quote recovery: recover mint quote, recover melt quote change.
- Local backup/export of tokens/proofs.
- Nostr mint-list backup when available.
- Clear warnings for replacing local wallet state or importing a backup over a nonzero balance.

## Contacts, Nostr, NWC, And Lightning Address

These are wallet extension surfaces, not replacements for core payment clarity.

- Contacts can route send ecash, payment requests, LNURL/Lightning payments, or Nostr DMs.
- Nostr can deliver payment requests, ecash payloads, profile identity, mint-list backup, or NWC commands.
- Lightning address can receive external Lightning payments that are auto-claimed as ecash.
- NWC should show the connection string/QR, trusted-app warning, permissions/limits, and online requirement.
