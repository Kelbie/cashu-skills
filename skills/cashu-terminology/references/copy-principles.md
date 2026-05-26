# Copy Principles

Use this reference when writing user-facing Cashu copy. Start from source-backed language first, then derive only when the existing projects do not cover the exact product moment.

## Source Pattern

Across protocol SDKs and wallet UIs, the stable split is:

- Protocol and SDK surfaces use exact nouns that exist in source: `proof`, `BlindedMessage`, `BlindSignature`, `mint quote`, `melt quote`, `PaymentRequest`, `PaymentRequestPayload`, `State`, `PaymentMethod`, `keyset`, `unit`.
- Primary wallet surfaces use short observed actions and rails: `Send`, `Receive`, `Ecash`, `Lightning`, `Mints`, `Top up`, `Pay`, `Transfer`.
- Recovery, diagnostics, and advanced settings can expose observed technical terms like `proofs`, `mint quote`, `melt quote`, `keysets`, and `NUTs`, but should name why the user is seeing them.
- Token copy appears when the serialized object matters: scanning, pasting, sharing, redeeming, decoding, or checking a `cashu...` string. Otherwise, observed wallet copy trends toward `ecash`.
- State copy should keep the state owner visible. Protocol or quote states can appear as `unspent proofs`, `spent proofs`, `mint quote paid`, or `melt quote pending`; local lifecycle words like `prepared`, `executing`, and `finalized` should stay in operation timelines, diagnostics, or developer-facing copy.

## Observed Copy Patterns

These are source-backed examples, not a mandate to use every phrase everywhere.

| User intent | Observed phrases | What to preserve |
| --- | --- | --- |
| Move bearer value to someone else | `Send ecash`, `Send Ecash`, `Share ecash`, `Share ecash token`, `Cashu token`, `Ecash Token` | Use `ecash` for the value and `token` when the encoded object is being shared, scanned, or displayed. |
| Accept a Cashu token | `Receive ecash`, `Receive Ecash`, `Receive to trusted mint`, `Receive to selected mint`, `Swap to a trusted mint`, `Token was redeemed.` | Use `receive` for the broad wallet action; use `redeemed` when the token has actually been claimed. |
| Ask someone to pay with ecash | `Payment request`, `Payment requests`, `Cashu Payment request`, `Create payment request`, `Send payment request`, `Payment request via {transport}` | Qualify as Cashu when Lightning, LNURL, NWC, HTTP, or NFC requests are nearby. |
| Add value through Lightning | `Top up wallet`, `Create invoice`, `Get Invoice`, `Create invoice to add funds`, `Share lightning invoice`, `Lightning invoice to pay` | Use invoice language for the Lightning artifact; keep `mint quote` for technical recovery or protocol details. |
| Pay out through Lightning | `Pay Lightning`, `Pay invoice`, `Pay a Lightning invoice`, `Pay bitcoin Lightning invoice with your ecash`, `Lightning invoice or address` | Use `Pay` in primary copy; reserve `melt` for protocol, SDK, and recovery details. |
| Move value between mints | `Transfer`, `Transfer between mints`, `inter-mint transfer`, `one trusted mint to another via the Lightning Network` | Use `transfer` for user-facing mint-to-mint movement; use `swap`, `mint quote`, and `melt quote` in the implementation detail. |
| Recover value | `Restore ecash`, `Recover mint quote`, `Recover melt quote change`, `unspent proofs`, `pending proofs`, `spent proofs` | Recovery copy can expose proofs and quotes because the user is fixing a technical state; qualify local lifecycle states separately. |
| Explain storage and custody | `This wallet stores ecash that only you have access to.`, `The ecash stored in the wallet is issued by the mint.`, `Local backup stores a copy of all your ecash` | Avoid implying app-operator custody; distinguish wallet-held ecash from mint-issued value. |

## Tone Rules

- Prefer an observed phrase when it fits the product moment. Do not add a polished phrase to this guide as recommended copy until it is found in source or explicitly approved.
- Use `ecash` as the value noun when the user-visible asset is the point.
- Use `Cashu token`, `ecash token`, or `Ecash Token` when the string, QR, NFC payload, or share object is the point.
- Use `proof` only when the audience needs protocol precision: debug views, recovery tools, SDK docs, database state, or mint APIs.
- Use `mint` as a noun freely, but explain it once as the server that issues and redeems ecash. Use `mint` as a verb only in technical or recovery copy unless local product copy already teaches that action.
- Use `receive` for broad wallet entry points. Use `redeemed` after a token has actually been claimed.
- Use `transfer` for user-facing mint-to-mint movement when the source already frames it that way. Use `swap` when the user is choosing a Cashu-level refresh/change operation or when the code/API actually performs a Cashu swap.
- Do not say `paid` alone. Tie it to the observed artifact or state: `invoice paid`, `Payment request has been successfully sent.`, `Token was redeemed.`, or the protocol quote state.
- If no observed phrase fits, write the copy as a candidate in the product surface, then backfill this guide only after it proves out.

## Good Copy Checks

- Can a user tell whether they are handling ecash, Lightning, LNURL, onchain Bitcoin, Nostr, NWC, NFC, or HTTP?
- Does the copy avoid implying custody by the app operator?
- Does `token` mean a serialized Cashu token and not an auth token, model token, or design token?
- Does `payment request` mean a Cashu payment request, not a BOLT11 invoice, LNURL pay request, HTTP request, or NWC command?
- Does the success copy identify the completed step rather than collapsing quote states?
- Does any state label say whether it belongs to a proof, quote, local operation, UI timeline, or external rail?
