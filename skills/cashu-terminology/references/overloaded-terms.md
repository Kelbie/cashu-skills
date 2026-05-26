# Overloaded Terms

Overloading is normal in Cashu systems. Do not rename a local term just because it is overloaded. First infer the intended meaning from the owner module, route/screen, rail, operation type, state machine, data shape, and nearby fields.

Qualify the term only when it crosses a boundary, appears in user copy, or remains ambiguous after that context check.

Boundary names are situational, not mandatory prefixes. If a method or field sits inside a Cashu-owned class, module, screen, store, SDK type, or parser branch, names like `paymentRequest` and `request` can be clear enough. Reach for `cashuPaymentRequest`, `encodedCashuPaymentRequest`, or `creq` when the reader would otherwise have to distinguish Cashu from Lightning, LNURL, HTTP, NWC, NFC, or generic request handling.

For state words, also identify whether the word is a protocol/wire term or a local abstraction. Uppercase proof and quote states such as `UNSPENT`, `PENDING`, `SPENT`, `UNPAID`, `PAID`, and `ISSUED` should stay tied to the owning protocol object; local lifecycle labels such as `prepared`, `executing`, `finalized`, `rolled_back`, and `reclaim` should be described as wallet/core abstractions.

| Word | Can mean | Infer from context | Qualify at boundaries as |
| --- | --- | --- | --- |
| `paymentRequest` | Cashu NUT-18 request, BOLT11 invoice, LNURL `payRequest`/`withdrawRequest`, BIP-321 field, HTTP request, NFC request, NWC invoice field. | Encoded prefix (`creq`, `lnbc`, `bitcoin:`), parser candidate kind, quote method, transport, caller module. | `cashuPaymentRequest`, `lightningInvoice`, `lnurlPayRequest`, `lnurlWithdrawRequest`, `httpRequest`, `nwcRequest`. |
| `request` | Mint quote invoice/address, melt target invoice/address, Cashu request object, HTTP request, NWC command request. | Parent object (`mintQuote`, `meltQuote`, `PaymentRequest`), method (`bolt11`, `bolt12`, `onchain`), network client boundary. | `quoteRequest`, `bolt11Invoice`, `bolt12Offer`, `onchainAddress`, `cashuPaymentRequest`, `httpRequest`. |
| `quote` | Mint quote id/response, melt quote id/response, app price quote, FX quote, operation record. | Operation owner (`mint`, `melt`, `fx`, `history`), fields like `request`, `fee_reserve`, `state`, `operationId`. | `mintQuote`, `meltQuote`, `quoteId`, `operationId`, `fxQuote`. |
| `token` | Serialized Cashu bearer data, proof bundle, API auth token, JWT, model token, design token. | Prefix (`cashu`), decoded shape (`mint`, `proofs`, `unit`), auth headers, UI theme/style context. | `cashuToken`, `ecashToken`, `encodedToken`, `authToken`, `jwt`, `proofs`; in user copy prefer `ecash` unless the encoded object matters. |
| `proof` | Cashu proof, DLEQ proof, Lightning payment proof/preimage, proof-of-payment callback data. | Fields (`amount`, `secret`, `C`, `id`), DLEQ object shape, Lightning `preimage`, callback payload. | `cashuProof`, `dleqProof`, `paymentPreimage`, `proofOfPayment`. |
| `input` / `output` | NUT-00 proofs/blinded messages, form inputs, function parameters, swap inputs/outputs, transaction outputs. | Protocol endpoint, UI form, function signature, swap/melt payload, Bitcoin transaction context. | `inputProofs`, `blindedOutputs`, `formInput`, `functionInput`, `txOutput`. |
| `secret` | Cashu proof secret, wallet seed/mnemonic, API secret, Nostr/private key material. | Object shape, storage location, key derivation code, auth config, user recovery screen. | `proofSecret`, `seedPhrase`, `mnemonic`, `apiSecret`, `nostrPrivateKey`. |
| `key` / `publicKey` | Mint signing key, Cashu keyset key, Nostr key, P2PK/P2BK key, API key, React/list key. | Prefix/encoding, keyset id, Nostr context, spending condition, HTTP auth, JSX/rendering context. | `mintSigningKey`, `keysetKey`, `nostrPubkey`, `p2pkPublicKey`, `apiKey`, `reactKey`. |
| `pending` | Protocol proof/quote state, send operation abstraction, mint operation abstraction, melt operation abstraction, UI loading, Lightning channel state. | State enum owner, history entry type, operation id, screen copy, channel/payment backend. | `pendingProof`, `pendingMintQuote`, `pendingMeltQuote`, `pendingSendOperation`, `loading`. |
| `paid` | Mint quote invoice paid, melt quote external payment paid, local transaction complete, UI success state. | Quote type, state machine owner, history entry, whether signatures/proofs were issued. | `mintQuotePaid`, `meltQuotePaid`, `invoicePaid`, `ecashIssued`, `paymentComplete`. |
| `state` | Protocol proof/quote state, local operation state, UI state, client state store, navigation state. | Enum owner, store/module name, route/screen, field name suffix. | `proofState`, `meltQuoteState`, `sendOperationState`, `uiState`, `navigationState`. |
| `prepared` / `executing` / `finalized` | Local operation lifecycle abstractions, not Cashu NUT proof or quote states. | Wallet/core operation owner, rollback/reclaim path, history/timeline mapping. | `preparedSendOperation`, `executingMintOperation`, `finalizedMeltOperation`, or keep the bare word inside the owning state machine. |
| `mint` | Mint server/operator, mint URL, verb to issue proofs, trusted/allowed mint row, mint quote flow. | Noun/verb grammar, URL field, table row/model, operation route, quote object. | `mintUrl`, `mintServer`, `mintProofs`, `allowedMint`, `mintQuote`. |
| `receive` | Redeem Cashu token, create Lightning invoice/top-up, receive Nostr message, receive NFC payload, receive swap/change. | Input payload kind, screen route, transport, operation owner, history type. | `receiveEcash`, `redeemToken`, `receiveLightning`, `createInvoice`, `receiveNostrMessage`, `receiveNfcPayload`. |
| `send` | Send ecash token, pay Lightning invoice, send Nostr DM, send HTTP request, send NFC response. | Destination rail, token creation, method call, transport, route/screen. | `sendEcash`, `shareEcash`, `payLightningInvoice`, `sendNostrMessage`, `sendHttpRequest`, `writeNfcToken`. |
| `address` | Lightning Address, Bitcoin onchain address, Nostr npub/nprofile, Nostr-derived Lightning address, mint URL. | String pattern (`user@domain`, `bc1`, `npub`, URL), parser candidate kind, payment method. | `lightningAddress`, `onchainAddress`, `npub`, `nprofile`, `mintUrl`. |
| `invoice` | BOLT11 invoice, displayed payment request, mint quote request string, NWC invoice field. | Prefix (`lnbc`, `lntb`), quote method, UI label, NWC method payload. | `bolt11Invoice`, `lightningInvoice`, `mintQuoteRequest`, `nwcInvoice`. |
| `method` | Cashu payment method, HTTP method, NWC method, class/object method. | Value set (`bolt11`, `bolt12`, `onchain`), HTTP client, NWC command, code member access. | `paymentMethod`, `httpMethod`, `nwcMethod`, `classMethod`. |
| `transport` | Cashu payment request transport, network transport, Nostr relay delivery, NFC handoff, HTTP POST. | NUT-18/NUT-26 object, URL/relay fields, NFC service, network layer. | `cashuRequestTransport`, `nostrTransport`, `nfcTransport`, `httpTransport`. |
| `fee` | Mint fee reserve, final Lightning fee, swap fee, onchain fee rate, app/service fee. | Melt quote fields, returned change, swap operation, onchain backend, pricing module. | `feeReserve`, `lightningFee`, `swapFee`, `onchainFeeRate`, `serviceFee`. |
| `change` | Cashu returned blind signatures for fee/change, swap remainder, Bitcoin transaction change output. | Melt response, swap operation, Bitcoin transaction builder. | `meltChange`, `swapRemainder`, `onchainChangeOutput`. |
| `backup` | Wallet seed phrase, mint list Nostr backup, local app DB export, channel backup. | Recovery screen, NUT-27/Nostr relay code, export/import module, Lightning node context. | `seedPhraseBackup`, `nostrMintBackup`, `walletExport`, `channelBackup`. |
| `transfer` | User-facing send, contact transfer, mint-to-mint movement, Bitcoin transaction, file transfer. | UI section, source/destination mints, token creation, external rail, network/file API. | `transferBetweenMints`, `sendEcash`, `bitcoinTransaction`, `fileTransfer`. |

## Boundary Naming Rules

| Boundary | Prefer | Avoid |
| --- | --- | --- |
| Cashu NUT-18/NUT-26 request | `paymentRequest` inside Cashu-owned context; `cashuPaymentRequest`, `encodedCashuPaymentRequest`, or `creq` at mixed boundaries | bare `paymentRequest` when Lightning, LNURL, HTTP, NWC, or NFC is nearby and no owner/type/prefix clue exists |
| Cashu request fulfillment | `paymentRequestPayload`, `cashuPaymentPayload` | `token` if it hides `id`, `memo`, `mint`, `unit`, `proofs` |
| BOLT11 invoice | `lightningInvoice`, `bolt11Invoice` | `request`, `paymentRequest` |
| BOLT12 offer | `bolt12Offer` | `invoice` |
| LNURL-pay response | `lnurlPayRequest`, `payRequest` in LNURL-only code | `cashuPaymentRequest` |
| LNURL-withdraw response | `lnurlWithdrawRequest`, `withdrawRequest` in LNURL-only code | `mintQuote` |
| Cashu token string | `cashuToken`, `ecashToken`, `encodedToken` | `token` in mixed auth/model contexts |
| Proof array | `proofs`, `cashuProofs`, `inputProofs`, `receivedProofs` | `tokens` |
| Mint quote id | `mintQuoteId`, `quoteId` in mint-only code | `invoiceId` |
| Melt quote id | `meltQuoteId`, `quoteId` in melt-only code | `paymentId` |
| Local operation id | `operationId` | `quoteId` |
| Fee reserve | `feeReserve`, `lightningFeeReserve`, `meltFeeReserve` | `fee` unless it is final |
| Final Lightning proof | `paymentPreimage` | `proof` |
| Onchain target | `onchainAddress`, `bitcoinAddress` | `address` |
| Nostr identity | `npub`, `nprofile`, `nostrPubkey` | `publicKey` if P2PK is also nearby |
| Mint list backup | `nostrMintBackup` | `backup` alone |
