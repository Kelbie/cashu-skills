# Source Refresh

Use this when the user asks to update the wallet UI-flow evidence behind this skill.

The goal is to refresh patterns without turning the skill into a catalog of named wallets. Keep named evidence in your scratch notes or in a separate user-requested audit artifact. Durable skill guidance should stay product-neutral.

## Refresh Steps

1. Identify the source set and date.
2. Record exact evidence anchors in scratch notes: local path, commit, URL, screenshot, route, component, translation key, or app version.
3. For each source, inspect user-facing entry points first: home actions, send/receive menus, scan behavior, history, mint management, settings, and onboarding.
4. For each feature, capture:
   - Entry point.
   - Screen order.
   - Primary labels and button copy.
   - Required inputs.
   - Mint selection behavior.
   - Trust/unknown-mint behavior.
   - Transport behavior.
   - Status/timeline behavior.
   - Error/recovery behavior.
5. Collapse named evidence into variants: amount-first, token-first, rail-menu, action-dock, trust/add, swap-to-trusted, auto-claim, multi-mint pay, and so on.
6. Update [Feature taxonomy](feature-taxonomy.md), [Flow patterns](flow-patterns.md), [Flow choice prompts](flow-choice-prompts.md), and [Copy and states](copy-and-states.md) only with generalized guidance.

## Evidence Matrix Template

Use this in scratch notes or a separate audit deliverable:

| Source | Feature | Entry | Order | Copy | State shown | Notable decision |
| --- | --- | --- | --- | --- | --- | --- |
| Source A | Send ecash | Send sheet | amount -> mint -> confirm -> token | Send Ecash, Continue, Share | prepared, finalized | Confirmation modal before token display. |

Then collapse it before editing the skill:

| Feature | Variant | Order | Primary copy | State shown | Main tradeoff |
| --- | --- | --- | --- | --- | --- |
| Send ecash | Amount-first confirmed token | amount -> mint -> confirm -> token | Send Ecash, Continue, Share | prepared, finalized | Clear operation safety, more steps than direct token generation. |

## What To Inspect

For source code:

- Routes/screens for wallet home, send, receive, payment request, Lightning receive/pay, mint management, history, settings, onboarding, NFC/POS, onchain, NWC, contacts/Nostr.
- Translation files for stable labels and errors.
- State machines or operation hooks for state names and action availability.
- Scan parsers and deep-link handlers for supported input types.
- History detail components for timeline/detail exposure.

For running apps:

- Capture the first screen, send menu, receive menu, scan result routing, send ecash, receive token, create invoice, pay invoice, mint list, history detail, backup/recovery.
- Note only user-visible behavior in the durable skill.

## Red Flags During Refresh

- A new source-specific phrase is added as a universal recommendation without comparing variants.
- Wallet names appear in `SKILL.md` or general references.
- Lightning invoices and Cashu payment requests are collapsed into one `payment request` concept.
- `pending` is copied without owner.
- Unknown mint behavior is treated as protocol-mandated instead of a product choice.
- Onchain confirmation progress is documented as Lightning state.

## Update Checklist

- New feature coverage added if a source has a feature missing from the taxonomy.
- Flow variants remain source-anonymized.
- Copy examples are grouped by feature and rail.
- Choice prompts ask implementers which variant they want.
- Technical states are attached to their owner.
- No named wallet guidance remains in the durable skill unless the user explicitly requested it.
