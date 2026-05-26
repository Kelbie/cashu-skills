---
name: cashu-wallet-ui-flows
description: Product-neutral Cashu wallet UI flow guide for designing, reviewing, documenting, or comparing send ecash, receive/redeem token, payment request, Lightning mint/top-up, Lightning melt/pay, mint management, mint transfer/rebalance, NFC/POS, onchain Bitcoin, scan/input, history, backup/recovery, Nostr/NWC/contact, and custody/trust flows. Use when a wallet implementer needs UI sequence options, copy patterns, state visibility, or questions that choose between observed Cashu wallet approaches.
---

# Cashu Wallet UI Flows

Use this skill when designing, reviewing, documenting, or comparing Cashu wallet product flows. It turns observed wallet UI behavior into product-neutral patterns. It should help an implementer choose the flow they want, not silently copy one existing wallet.

Keep durable guidance source-anonymized. Do not name specific wallets or apps in this skill or its references unless the user explicitly asks for a named source audit. Phrase evidence as "observed pattern", "common approach", or "variant".

## Workflow

1. Identify the feature family: ecash send, ecash receive, payment request, Lightning receive, Lightning pay, transfer/rebalance, mint management, scan/input, NFC/POS, onchain, history, backup/recovery, contacts/Nostr/NWC, or onboarding.
2. Decide the flow order before writing screens: amount-first, target-first, rail-first, mint-first, or token-first. Different wallets use different orders for valid reasons.
3. Ask the implementer the smallest set of flow-choice questions from [Flow choice prompts](references/flow-choice-prompts.md). Do not assume the product wants the same sequence as another wallet.
4. Use [Feature taxonomy](references/feature-taxonomy.md) to verify the feature surface is complete.
5. Use [Flow patterns](references/flow-patterns.md) for verbose UI descriptions, variants, state exposure, and expected screen progression.
6. Use [Copy and states](references/copy-and-states.md) for labels, button copy, rail nouns, status words, and warnings.
7. If the user asks to refresh the source survey, follow [Source refresh](references/source-refresh.md) and collapse new findings back into product-neutral variants.

## Principles

- Start with the user action and rail: send ecash, receive ecash, request ecash, pay Lightning, receive via Lightning, pay onchain, receive onchain.
- Keep the mint visible whenever mint choice changes safety, availability, fees, or custody.
- Make the encoded object visible when the object itself is the handoff: Cashu token, payment request, Lightning invoice, onchain address, NFC payload, or NWC connection string.
- Preserve state ownership: proof/token state, quote state, operation state, external rail state, and UI loading state are different things.
- Unknown mint handling is a product decision. The main valid choices are reject, trust/add then redeem, receive later, or swap to a trusted mint.
- Payment request handling is two-sided. Creation asks "how can the payer return ecash?" Paying asks "which transport should deliver the token?"
- Advanced flows can reveal protocol terms, but primary UI should favor plain user actions unless the user is debugging or recovering value.

## Clean Comparison Format

When comparing flows, use this format instead of source-name-by-source-name prose:

| Feature | Variant | Order | Primary copy | State shown | Main tradeoff |
| --- | --- | --- | --- | --- | --- |
| Send ecash | Amount-first token | Amount -> mint -> confirm -> token | `Send Ecash`, `Create Token`, `Copy`, `Share` | prepared, token ready, redeemed/spent check | Simple handoff, but token object becomes central. |
| Receive ecash | Token-first redeem | paste/scan -> preview -> trust/mint decision -> redeem | `Receive Ecash`, `Redeem Ecash`, `Receive later` | decoded, trusted/unknown, redeemed, invalid/spent | Clear safety moment, more steps. |

Use feature variants, not wallet names, as the comparison axis.

## References

- [Feature taxonomy](references/feature-taxonomy.md): complete feature families and required UI questions.
- [Flow choice prompts](references/flow-choice-prompts.md): questions to ask implementers before designing a flow.
- [Flow patterns](references/flow-patterns.md): verbose product-neutral flow descriptions.
- [Copy and states](references/copy-and-states.md): labels, status copy, and state ownership.
- [Source refresh](references/source-refresh.md): how to update this skill from a new wallet survey without making it product-specific.
