---
name: cashu-terminology
description: Product-neutral Cashu terminology guide for naming, reviewing, documenting, or explaining ecash, proofs, tokens, mint/melt/swap flows, quotes, payment requests, Lightning, LNURL, onchain Bitcoin, Nostr, NWC, NFC/POS, wallet-core state, and mint/server terms. Use when Cashu protocol, client UI, wallet core, and server code use overloaded or boundary-specific language.
---

# Cashu Terminology

Use this skill when wording, renaming, documenting, reviewing, or debugging Cashu-related flows. The goal is precise language across protocol, wallet/client, mint/server, wallet-core lifecycle, user copy, Lightning/LNURL, onchain Bitcoin, Nostr, NWC, NFC/POS, and payment-request transports.

Keep this skill product-neutral. Do not copy app or product names into durable guidance unless the user explicitly asks for a source-specific audit.

## First Principles

1. Cashu value is bearer ecash issued by mints and held by wallets as proofs. Users act on `ecash`; protocol code acts on `proofs`; strings, links, and QR codes usually carry `Cashu tokens` or `Cashu payment requests`.
2. A mint is an issuer and settlement service. A wallet is the user's agent. Avoid copy that implies the app operator has custody, that a mint is a wallet account, or that all ecash is one global balance.
3. The same movement has different names by layer: issue or mint proofs, redeem or receive a token, melt to pay an external target, and swap to refresh/change proofs. Pick the term for the layer you are editing.
4. Rails are not interchangeable. A Cashu payment request asks for ecash; a Lightning invoice asks for a Lightning payment; an LNURL request negotiates a later invoice; a `bitcoin:` URI can carry several instructions.
5. Good UI copy names the user action and rail first. Technical details can reveal exact protocol nouns after the user has enough context.

## Workflow

1. Classify the term by owner and boundary: protocol, SDK/core, mint/server, wallet UI, NFC/POS, Lightning/LNURL, onchain, Nostr/NWC, or payment transport.
2. For overloaded terms, infer the intended meaning from owner module, route/screen, rail, operation type, state machine, data shape, and nearby fields before renaming anything.
3. Preserve local terms inside their owning module when the meaning is clear. Qualify terms at shared boundaries, in user copy, and anywhere ambiguity leaks.
4. Prefer protocol wording in specs, shared APIs, SDKs, and server code: `proof`, `proofs`, `BlindedMessage`, `BlindSignature`, `mint quote`, `melt quote`, `swap`, `keyset`, `unit`.
5. Prefer user-facing wording in primary UI: `ecash`, `Cashu token`, `send ecash`, `receive ecash`, `redeem token`, `Lightning invoice`, `onchain address`.
6. Before copying local variable names into UI copy, use the copy ladder in [Copy principles](references/copy-principles.md).
7. Keep adjacent rails distinct. A Cashu payment request, BOLT11 invoice, LNURL request, BIP-321 parameter, HTTP request, NWC request, and NFC request can all be "requests" in local context.

## References

- [Copy principles](references/copy-principles.md): first-principles UI copy and action labels.
- [Core terms](references/core-terms.md): Cashu primitives, minting, melting, swapping, UI wording.
- [Payment rails and requests](references/payment-rails-and-requests.md): Cashu payment requests, Lightning, LNURL, onchain, Nostr, NWC, NFC/POS.
- [States and layers](references/states-and-layers.md): protocol states, local operation states, client/server/UI split.
- [Overloaded terms](references/overloaded-terms.md): terms that are naturally overloaded and how to infer context.
- [Source refresh](references/source-refresh.md): how to update evidence without turning the skill product-specific.

## Red Flags

- `paymentRequest` without an owner, rail, type, prefix, or data-shape clue.
- `token` used for a single proof, API/auth bearer, model token, design token, or serialized Cashu data without context.
- `paid` without quote type: mint quote `PAID` is not `ISSUED`; melt quote `PAID` means external payment completed.
- `pending` without a noun: proof state, quote state, send operation, mint operation, melt operation, UI loading state, or Lightning channel state.
- User copy that says `receive`, `send`, `request`, `invoice`, or `address` without identifying ecash, Lightning, onchain, LNURL, Nostr, NWC, or Cashu payment request context.
