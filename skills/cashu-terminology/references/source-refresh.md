# Source Refresh

Use this reference when updating the skill from current source evidence.

## Evidence Policy

- Keep durable guidance product-neutral.
- Record app/product names only when the user asks for a source-specific audit or evidence ledger.
- Treat local source observations as examples of layer behavior, not as product-specific rules.
- Re-check live source before relying on drift-prone claims.
- Exclude unrelated non-Cashu product terminology from Cashu guidance.

## Source Priority

1. Cashu protocol specs: `github.com/cashubtc/nuts`.
2. Bitcoin URI/onchain standards: BIP-321, BIP-21 compatibility, BIP-353, BIP-352 Silent Payments, bech32/bech32m, BIP-39.
3. LNURL standards: LNURL-pay, LNURL-withdraw, LNURL-auth, Lightning Address, callback, `pr`.
4. Cashu SDKs, wallet-core libraries, mint/server implementations.
5. Wallet UI, NFC/POS, NWC, and payment-request flows.
6. Local audit docs as evidence ledgers only.

## Useful Search Patterns

Search relevant local repos with `rg` before editing terms:

```text
paymentRequest|cashuPaymentRequest|PaymentRequest|creq|CREQ
lightningInvoice|bolt11|invoice|BOLT11|BOLT12
lnurl|LNURL|payRequest|withdrawRequest|Lightning Address
proof|proofs|BlindedMessage|BlindSignature|keyset|DLEQ
mint quote|mintQuote|melt quote|meltQuote|fee_reserve|feeReserve
pending|paid|issued|spent|unspent|prepared|executing|finalized|reclaim|rollback
token|Cashu token|ecash|send ecash|receive ecash|redeem
nostr|npub|nprofile|relay|NWC|pay_invoice|make_invoice|lookup_invoice
onchain|on-chain|bitcoin:|BIP-321|address|fee rate
NFC|POS|payment request|allowed mints|trusted mint|preferred mint
```

## Refresh Workflow

1. Identify the boundary: protocol, SDK/core, mint/server, wallet UI, NFC/POS, Lightning/LNURL, onchain, Nostr/NWC.
2. Read the protocol/standard first if the term has a wire meaning.
3. Sample at least one implementation or UI surface for the target boundary.
4. Add durable wording only when it generalizes across sources or is mandated by protocol.
5. If a term is overloaded, add it to `overloaded-terms.md` with inference clues instead of banning the term.
6. Keep `SKILL.md` under 200 lines; move detail into references.
7. Run validation checks before finishing:

```sh
find skills/cashu-terminology -name '*.md' -print
wc -l skills/cashu-terminology/SKILL.md skills/cashu-terminology/references/*.md
rg -n "[[:blank:]]$" skills/cashu-terminology
perl -ne 'print "$ARGV:$.:$_" if /[^\x00-\x7F]/' skills/cashu-terminology/**/*.md skills/cashu-terminology/SKILL.md
```
