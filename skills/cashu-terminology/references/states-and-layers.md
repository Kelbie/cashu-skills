# States and Layers

Use this reference when a term changes meaning between protocol state, local operation state, mint/server code, wallet core, and UI.

## Protocol and Local States

| State word | Protocol meaning | App/core meaning |
| --- | --- | --- |
| `UNSPENT` | Proof state: proof can still be spent. | Wallet may still mark local proof reserved/pending for UX. |
| `PENDING` proof | Proof is being processed and should not be reused until it resolves. | Wallets may also use `pending` for sent token operations and UI timelines. |
| `SPENT` | Proof secret is in the mint spent list. | A sent token being spent by recipient may move local transaction to complete/finalized. |
| `UNPAID` mint quote | External payment has not been paid. | UI may still show "waiting for payment". |
| `PAID` mint quote | External payment is paid, but proofs may not be issued. | Wallets may next run mint/claim/finalize. |
| `ISSUED` mint quote | Mint signatures/proofs have been issued. | User-facing "ecash received" is safe after proofs are stored. |
| `UNPAID` melt quote | External target has not been paid. | Wallet may still allow rollback or cancellation depending on local operation state. |
| `PENDING` melt quote | External payment is in progress. | Pollers/watchers may recover change or update history later. |
| `PAID` melt quote | External payment completed. | User-facing "Lightning paid" is safe; returned change still may need recovery. |
| `prepared` | Not a protocol state; local operation has reserved/assembled data but has not finalized. | Can often be canceled/rolled back. |
| `executing` | Not a protocol state; local operation is actively calling the mint. | Avoid mapping directly to `PENDING` unless documenting a UI timeline. |
| `pending` operation | Local operation has created a token or is waiting for final quote/proof state. | Qualify as `send operation pending`, `mint operation pending`, or `melt operation pending`. |
| `finalized` | Local operation complete. | If mapped to quote or payment success, keep that mapping explicit. |
| `rolled_back` / `reclaim` | Local recovery/cancel path. | Not a NUT state. |

## Client, Core, Server, UI

| Word | Protocol/server use | Wallet/core use | UI/POS use |
| --- | --- | --- | --- |
| receive | Mint receives proofs as inputs; wallet swaps token proofs to fresh proofs. | `receive(token)` usually means redeem token into stored proofs. | Observed copy includes `Receive ecash`, `Receive Ecash`, `Receive Lightning`, `Create invoice`, and `Get Invoice`; choose by rail. |
| send / pay | Wallet constructs token or melt request; mint validates proofs and may pay external target. | Send operations create pending tokens and monitor proof state. | Observed copy includes `Send ecash`, `Share ecash`, `Pay Lightning`, `Pay invoice`, and `Pay a Lightning invoice`; choose by rail. |
| request | Field name for BOLT11 invoice, BOLT12 offer, onchain instruction, or Cashu payment request depending on method. | Variable names like `paymentRequest` are often overloaded. | Say `Lightning invoice`, `Cashu payment request`, `LNURL pay request`, or `onchain address`. |
| quote | Protocol quote id and server quote response. | Local operation may have `quoteId`, `operationId`, and history entry id. | `Invoice`, `payment request`, `estimate`, `quote details`, or `recover mint quote` depending on screen. |
| pending | Proof/quote state. | Local operation/timeline state. | "Waiting", "not claimed yet", "still pending", with the noun. |
| paid | Quote state with different meaning for mint vs melt. | History state may map `finalized` to payment success. | Use "invoice paid", "ecash issued", or "payment complete". |
| address | Lightning Address, Nostr address/key, Bitcoin onchain address, Nostr-derived Lightning address, or mint URL. | Detector/route type. | Always qualify. |

## Layer Language

| Layer | Prefer | Preserve internally |
| --- | --- | --- |
| Protocol/spec | `proof`, `BlindedMessage`, `BlindSignature`, `mint quote`, `melt quote`, `swap`, `unit`, `keyset`, NUT names. | Wire object names, endpoint names, quote/proof states. |
| SDK/core | `operationId`, `quoteId`, `proofs`, `prepared`, `executing`, `pending`, `finalized`, `reclaim`, `rollback`. | Local lifecycle state and exact owner module naming. |
| Mint/server | `mint URL`, `method`, `unit`, `keyset`, `pending proofs`, `cached response`, `auth`, `quote state`. | Database/model enum names when documenting storage or APIs. |
| Wallet UI | `ecash`, `Cashu token`, `Send ecash`, `Receive ecash`, `Top up wallet`, `Create invoice`, `Get Invoice`, `Lightning invoice`, `Lightning Address`, `seed phrase`, `Transfer between mints`. | Advanced details screens can expose proof/keyset/counter details when accurately scoped. |
| NFC/POS | `Cashu payment request`, `Cashu requests`, `Lightning invoices`, `Scan or tap to pay`, `trusted mint`, `unknown mint`. | Encoded `creq`, NFC tag, and token response terms in implementation surfaces. |
| Nostr/NWC | `Nostr relays`, `Nostr mint backup`, `Nostr Wallet Connect`, `Lightning address`, `pay_invoice`, `make_invoice`, `lookup_invoice`. | Protocol method names and relay/auth details. |

## Conflict Resolution

When sources disagree:

1. Use `github.com/cashubtc/nuts` for protocol/wire behavior and state names.
2. Use Bitcoin BIPs and LNURL LUDs for Bitcoin URI and LNURL meanings.
3. Use the owning SDK/module terminology inside that SDK/module's own code.
4. For shared app or library code, use rail-qualified names even if an upstream library uses generic `request` or `PaymentRequest`.
5. For user copy, prefer wallet-client consensus: `ecash`, `Cashu token`, `redeem`, `send`, `receive`, `Lightning invoice`, `Lightning Address`, `onchain address`.
6. If a design doc and implemented code differ, state which one you mean.
