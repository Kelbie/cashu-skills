# Flow Choice Prompts

Use these questions at the start of a Cashu wallet UI task. Ask only the questions that affect the requested feature.

## Universal Questions

1. Which rail is this feature for: ecash token, Cashu payment request, Lightning, LNURL, onchain Bitcoin, NFC, Nostr, NWC, or multiple rails?
2. What should the user choose first: action, rail, amount, recipient/target, mint, or scan/input?
3. Should the flow be a quick action sheet, a full-screen wizard, a modal confirmation, or a history/detail screen?
4. How much protocol detail should be visible: plain copy only, advanced details on demand, or always-visible quote/proof/debug information?
5. What should happen if the mint is unknown, offline, unsupported, or has insufficient balance?

## Send Ecash

- Do you want amount-first sending, recipient-first sending, or token-generation form first?
- Should the user choose a mint before amount, from the header, or only when multiple mints can pay?
- Is proof/coin selection exposed, hidden, or advanced-only?
- Should the token be shown immediately after prepare, only after a confirm step, or after a background operation finalizes?
- Which handoff methods are first-class: copy, share sheet, QR, link, emoji, NFC, Nostr DM, HTTP POST?
- Should the wallet track redemption and offer `Check Status`, `Cancel Transaction`, or `Reclaim`?
- Should offline send be supported, and if yes does it create a pending token or just show a warning?
- Is P2PK locking a primary field, a scan-only option, or hidden under advanced controls?

## Receive Ecash

- Should receive start with paste, scan, clipboard auto-detect, or a receive hub?
- Should the decoded token preview show amount, mint, memo, lock status, and token text before redeem?
- Unknown mint policy: reject, trust/add then redeem, swap to a trusted mint, receive later, or ask every time?
- Should receiving work offline as a prepared state?
- Should locked tokens be redeemable later if the wallet does not have the key?
- Should success copy say `Received`, `Redeemed`, `Claimed`, or include the amount and mint?

## Cashu Payment Request Creation

- Is amount required or optional?
- Should the request accept any mint, selected mints, or only one receive mint?
- Should the request include a transport, and which one is default: Nostr, HTTP POST, NFC, in-band/manual?
- Should the receiver's P2PK key be included?
- Is the request single-use or reusable?
- Should the final screen show QR, raw request string, payment ID, transport, and copy/share actions?

## Cashu Payment Request Payment

- Should payment requests be handled from scan, paste, deep link, NFC, contact, or all of these?
- If the request amount is missing, should the payer enter amount before or after selecting a mint?
- If no allowed mint is in the wallet, should the UI suggest adding a mint, transferring to an allowed mint, or reject?
- If transport delivery fails, should the wallet display the token for manual delivery?
- Should Nostr relay connection state or HTTP endpoint validity be shown before confirmation?

## Lightning Receive

- Is the user-facing goal `Receive Lightning`, `Top up`, `Get Ecash`, `Create invoice`, or `Issue Ecash`?
- Should the wallet issue ecash automatically when the invoice is paid?
- Should invoice expiry, quote ID, and mint be visible by default?
- Should the invoice be copy/share/QR only, or should contacts/Nostr send be built in?
- Does the wallet support Lightning address auto-claim, and where does that live?

## Lightning Pay

- Should the flow begin with a target input, scanner, contact, or amount field?
- Should LNURL/Lightning-address amount entry be a second step or inline?
- Should the mint be auto-selected, selected in the header, selected in a modal, or split across multiple mints?
- Should fee and total be shown before confirm?
- Should success navigate to a compact success screen or stay on a detailed transaction view?
- How should async/pending payments be described, and what can the user do next?

## Mint Management

- Is adding a mint manual URL entry, search/discovery, recommendation, token-driven, or all of these?
- What trust signal is shown before adding?
- Can users block receiving from a mint without deleting it?
- Can users delete a mint with nonzero balance?
- Are proof checks, spent-proof cleanup, and reserved-proof reset exposed?
- Does mint info show NUT support, limits, contact, MOTD, reviews, or operator identity?

## Onchain

- Is onchain supported for receive, send, both, or neither?
- Should onchain live beside Lightning under receive/pay, or have separate routes?
- Does the timeline show mempool detection and confirmation progress?
- Does copy say `onchain address`, `Bitcoin address`, or `bitcoin:` URI?
- How many confirmations are required before issuing ecash?

## NFC/POS

- Is the device paying a terminal, acting as the terminal, or writing/reading token cards?
- Is the NFC payload a Cashu payment request, Cashu token, Lightning invoice, or another URI?
- Should the NFC session remain open through read, prepare, and write?
- What fallback exists when NFC is unsupported or disabled?
- Should POS show QR, NFC, or both?

## History And Recovery

- Which states belong in the timeline versus details?
- Should pending sends be cancellable or reclaimable from history?
- Should quote recovery be visible in normal recovery or developer tools?
- Should exported token/proof data be user-facing, support-only, or hidden?
- Should the history list group by type, mint, day, or show a single activity feed?
