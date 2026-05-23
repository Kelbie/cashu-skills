# Payment Rails and Requests

Use this reference when Cashu terms meet Lightning, LNURL, onchain Bitcoin, Nostr, NWC, NFC/POS, or payment-request transports.

## Cashu Payment Requests

| Term | Meaning |
| --- | --- |
| Cashu payment request | NUT-18 receiver-created request. Sender creates a matching Cashu token and delivers it by the requested transport. |
| `creqA` | NUT-18 base64url-encoded Cashu payment request. |
| `creqb` / `CREQB` | NUT-26 Bech32m/TLV encoding of a Cashu payment request. Uppercase is preferred for QR compatibility. |
| `paymentRequest` | Only acceptable unqualified inside a local Cashu-payment-request module. At boundaries prefer `cashuPaymentRequest`. |
| payment request payload | NUT-18 sender payload containing `id`, `memo`, `mint`, `unit`, and `proofs`. Distinct from the request itself. |
| transport | Delivery method in the Cashu payment request. NUT-18/NUT-26 use `nostr`, `post`, or in-band/no transport. |
| Nostr transport | Cashu payment request transport using a Nostr target, usually encoded as raw x-only pubkey, `npub`, or `nprofile` with relay tags. |
| HTTP POST transport | Sender posts the payment payload to the request target. |
| in-band | No transport field; sender returns/displays the token through the current channel. |
| NFC handoff | Local exchange where a device reads a Cashu payment request and writes a Cashu token response back through NFC. Do not call the returned token a payment request. |
| NUT-24 `X-Cashu` | HTTP 402 flow where a server asks for Cashu payment via `X-Cashu` and the client retries with a token. |

Naming rule: use `cashuPaymentRequest` for NUT-18/NUT-26, `paymentRequestPayload` for the fulfillment payload, and `paymentRequest` only where the Cashu meaning is already explicit.

## Bitcoin, Lightning, LNURL, Onchain

| Term | Meaning | Naming rule |
| --- | --- | --- |
| Bitcoin / BTC | Base network and asset. Cashu can represent BTC value as sat-denominated ecash and settle over Lightning or onchain methods. | Do not use `bitcoin` as the only label for a Cashu balance. |
| sat / sats / satoshi | Common unit for Bitcoin-denominated Cashu flows. | Prefer `sat` in protocol/code units and `sats` in UI copy. |
| msat / millisat | Lightning/LNURL amount unit. | Convert explicitly when Cashu amount is in sats. |
| BOLT11 invoice | Encoded Lightning invoice. Appears in fields named `request`, `paymentRequest`, or `invoice`. | Prefer `lightningInvoice` or `bolt11Invoice`. |
| BOLT12 offer | Lightning offer. NUT-25 uses BOLT12 for mint/melt payment methods. | Verify support before presenting as available. |
| Lightning Address | Human-readable address backed by LNURL-pay (`name@domain`). | Do not call all `user@domain` strings Nostr or email. |
| LNURL | Bech32-encoded HTTPS/Onion URL or raw scheme such as `lnurlp://`/`lnurlw://` in some LUDs. | Use `lnurl`, `lnurlPay`, or `lnurlWithdraw` in code. |
| LNURL-pay `payRequest` | LUD-06 response that later returns a BOLT11 invoice in `pr`. | Do not call it Cashu `paymentRequest`. |
| LNURL-withdraw `withdrawRequest` | LUD-03 response where wallet submits a BOLT11 invoice in `pr`. | Do not confuse with Cashu mint quote. |
| LNURL-auth | LUD-13 linking-key auth flow. | Separate from Nostr auth and NUT-21 mint auth. |
| `successAction` | LNURL-pay post-payment action, sometimes decrypted with payment preimage. | Not a Cashu transaction state. |
| BIP-321 `bitcoin:` URI | Multi-instruction Bitcoin URI. Can carry `lightning`, `lno`, `creq`, onchain address/instructions, and required `req-*` params. | When multiple supported instructions are present, choose by current flow and support matrix. |
| BIP-21 | Legacy Bitcoin URI superseded by BIP-321. | Mention only for compatibility/history. |
| BIP-353 payment name | DNSSEC-backed human-readable Bitcoin payment name resolving to a `bitcoin:` URI. | Do not label as Lightning Address if it may resolve to multiple Bitcoin instructions. |
| Silent Payments | BIP-352 static onchain payment addresses. | Onchain payment instruction, not Lightning or Cashu. |
| onchain / on-chain | Bitcoin base-layer payment method/address/transaction. | In code use established local style; in UI prefer `onchain address` if local copy already uses it. |
| outpoint / txid / confirmation | Onchain transaction terms. | Do not use for Lightning invoice payment state. |
| fee rate / fee tier | Onchain fee selection terms. | Distinct from Lightning fee reserve. |

## Nostr and NWC

| Term | Meaning | Naming rule |
| --- | --- | --- |
| Nostr | Relay-based social/identity protocol used for profiles, relays, encrypted messages, NWC, mint backup, and Cashu request transport. | Do not use as a generic "contact" term unless identity is actually Nostr-backed. |
| `npub` | Bech32-encoded Nostr public key. | Not automatically a Cashu P2PK key. |
| raw x-only pubkey | 32-byte Nostr/secp256k1 pubkey. | NUT-26 Nostr target may encode as raw bytes, `npub`, or `nprofile`. |
| `nprofile` | Nostr profile pointer with relay hints. | Preserve relay hints when transport delivery needs them. |
| relay | Nostr relay URL. | Do not call it a mint or transport unless context is Nostr transport. |
| NIP-17 / NIP-59 | Private/gift-wrapped message patterns used by wallet apps. | Different from NUT-17 Cashu WebSocket subscriptions. |
| NIP-44 | Encryption used by NUT-27 mint backup. | Do not confuse with NIP-04 legacy direct messages. |
| NUT-27 mint backup | Encrypted mint list backup to Nostr relays as kind `30078`, derived from mnemonic with Cashu-specific domain separation. | Distinguish from NUT-18 Nostr transport. |
| NIP-98 auth | HTTP auth event used by some Nostr-integrated services. | Distinct from NUT-21 Clear Auth and OIDC CATs. |
| Nostr-derived Lightning address | User-facing address derived from Nostr identity and backed by a service or mint integration. | Keep distinct from raw `npub`, Nostr relay URL, and mint URL. |
| Nostr Wallet Connect / NWC | Nostr-based remote wallet command protocol with methods like `pay_invoice`, `make_invoice`, and `lookup_invoice`. | Not the same as Cashu payment request transport. |

## Onchain Cashu Methods

Use `onchain` as a Cashu payment method only when the mint/library actually supports it.

| Term | Meaning |
| --- | --- |
| onchain mint quote | Mint quote whose `request` is an onchain Bitcoin payment instruction/address rather than a Lightning invoice. |
| onchain melt quote | Melt quote that pays an onchain Bitcoin target. |
| onchain wallet backend | Payment backend for onchain mint/melt support, including address derivation, chain sources, fee estimation, and confirmation tracking. |
| onchain address | Bitcoin base-layer destination/address displayed to users. |
| onchain payment observed | App-level observation of mempool/confirmed payment to an address. |
| onchain sending not supported | Valid when the app detects an onchain address but the current trusted mint/client flow cannot execute the method. |
