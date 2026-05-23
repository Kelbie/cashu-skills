# Core Terms

Use this reference for Cashu primitives, minting, melting, swapping, and user-facing wallet wording.

## Source Priority

1. Protocol/wire behavior: `github.com/cashubtc/nuts`.
2. SDK/core API: owning module/library names when they directly model protocol objects.
3. Mint/server behavior: quote, method, unit, keyset, proof, pending proofs, endpoint names.
4. Wallet UI: user-facing rail and action words.

## Cashu Primitives

| Term | Use for | Avoid/conflict |
| --- | --- | --- |
| Cashu | The ecash protocol and ecosystem. | Do not use as a currency name. |
| ecash | User-facing bearer value issued by a Cashu mint. | Do not use when the object is specifically a serialized token or individual proof. |
| mint | Service/operator that signs blinded messages, issues signatures/proofs, and validates or spends proofs. | Also a verb in "mint proofs"; qualify when noun and verb collide. |
| wallet | Client/app/library that manages proofs and talks to mints. | Do not imply custody by the app operator. |
| unit | Unit for keysets and operations, commonly `sat`; keep explicit in generic APIs. | Do not assume all Cashu is BTC/sat when code supports units. |
| amount | Numeric value in a unit. LNURL often uses millisats; Cashu wallet UI often shows sats. | Do not mix sats and millisats silently. |
| keyset | Mint signing-key set for a unit and amount set. Proofs reference keyset IDs. | Do not call this a wallet account. |
| proof | Bearer spend primitive with amount, keyset id, secret, and signature point `C`. | Do not call one proof a token in precise API docs. |
| proofs | List of proof objects composing balance, token contents, or payment inputs. | Do not log raw proofs or secrets. |
| secret | Proof secret; ordinary proofs are spendable by anyone who knows it unless a spending condition is enforced. | Do not confuse with wallet seed phrase. |
| `Y` | Hash-to-curve of a proof secret, used for proof state lookup. | Do not expose as user-facing identity. |
| `C` | Unblinded signature point on a proof secret. | Distinct from `C_`. |
| `B_` | Blinded message point in a `BlindedMessage`. | Distinct from proof `C`. |
| `C_` | Blinded signature point in a `BlindSignature`. | Distinct from unblinded proof `C`. |
| BlindedMessage | Wallet-created blinded request to a mint; NUT-00 also calls it an `output`. | In UI, avoid "output" unless advanced/debug. |
| BlindSignature | Mint response over a blinded message; NUT-00 also calls it a `promise`. Wallet unblinds it to become a proof. | Do not call it a final proof before unblinding. |
| token | Serialized bearer bundle of one or more proofs, usually `cashu...`, `cashu:...`, or an object with `mint`, `proofs`, `memo`, and `unit`. | Ambiguous with API/auth/model/design tokens. Prefer `Cashu token` or `ecash token` in UI. |
| token V3 / token V4 | Cashu token serialization formats. V4 is current; V3 is legacy/deprecated. | Do not use version to mean app transaction version. |
| DLEQ | Cryptographic proof that a blind signature/proof matches mint keys. Used for offline token verification. | Not a spendable proof and not a payment preimage. |
| P2PK | Pay-to-Public-Key proof lock requiring a Schnorr signature/witness to spend. | Do not assume a Nostr `npub` is directly a Cashu P2PK key. |
| P2BK | Pay-to-Blinded-Key from NUT-28. Blinds receiver keys, including Nostr x-only pubkeys with SEC1 prefix handling. | Do not call it ordinary P2PK in protocol docs. |
| spending condition | NUT-10 secret format that lets mints enforce locks such as P2PK/HTLC. | If a mint does not support a kind, a proof can be treated as ordinary. |
| witness | Signature/preimage data supplied to satisfy a spending condition. | Do not use for generic "evidence" unless cryptographic context is clear. |
| seed phrase / mnemonic | User-facing wallet recovery phrase, usually BIP-39. | Do not call proof `secret` a seed phrase. |
| derivation counter | Deterministic secret counter per keyset. | Internal recovery/SDK term, not UI copy. |

## Minting, Melting, Swapping

| Term | Meaning | States and cautions |
| --- | --- | --- |
| mint quote | Quote for issuing new ecash after paying the mint's payment request. NUT-04 `quote` is a unique quote id; `request` is usually the external payment request. | BOLT11 mint quote states are `UNPAID`, `PAID`, `ISSUED`. `PAID` means the invoice is paid; `ISSUED` means signatures/proofs were issued. |
| mint | Verb: issue blind signatures for outputs after a quote is payable. Wallet unblinds signatures into proofs. | Do not say "minted" if the quote is only `PAID`. |
| minting / issue ecash | User-facing receive/top-up flow. | Protocol docs should still say `mint quote` and `mint`. |
| melt quote | Quote for spending proofs through a mint to pay an external target. | Melt quote states are `UNPAID`, `PENDING`, `PAID`. `PENDING` means external payment is in progress; `PAID` means completed. |
| melt | Spend proofs to pay an external payment request, commonly a BOLT11 invoice. | UI should usually say `pay Lightning invoice` or `withdraw`, not `melt`, unless technical. |
| fee reserve | Amount reserved by a mint for possible routing/payment fees on melt. | Not the final fee. Distinguish from final fee and returned `change`. |
| change | Blind signatures returned for over-reserved melt fees or swap remainder. | Not a Bitcoin transaction change output. |
| payment preimage | Lightning secret returned when a BOLT11 invoice is paid. | Not a Cashu proof, even if a local field is named `proof`. |
| swap | Cashu operation that invalidates input proofs and returns signatures for new outputs. Used for splitting, consolidating, redeeming a token, reclaiming, P2PK locking, and change. | Do not use `swap` for arbitrary exchange between rails unless the flow actually does Cashu swap plus another rail operation. |
| send ecash | Create a Cashu token for someone else. May use exact local proofs without a mint call if denominations match. | Sent proofs often become local `pending` until reclaimed, rolled back, or observed as spent. |
| receive ecash / redeem token | Swap incoming token proofs into fresh wallet proofs. | `receive` can also mean receive Lightning or request a payment; qualify the rail. |
| pending proofs | Proofs reserved or being processed by the mint. | NUT-07 proof state `PENDING` is protocol state; wallet DB `isPending`/`reserved` can be local state. |
| batched minting | NUT-29 all-or-nothing minting of multiple paid quotes with `quotes`, `quote_amounts`, `outputs`, `signatures`. | Same method and unit; do not describe as generic batch payment. |
| cached responses | NUT-19 mint/server cache for successful responses to critical operations. | Not app cache or HTTP CDN cache. |
| WebSocket subscriptions | NUT-17 quote/proof updates by subscription kind, e.g. `bolt11_mint_quote`, `bolt11_melt_quote`, `proof_state`. | `subId` is wallet-generated; do not confuse with JSON-RPC request id. |

## UI Copy

Prefer in primary UI:

- `Send ecash`
- `Receive ecash`
- `Redeem token`
- `Get invoice`
- `Issue ecash`
- `Pay Lightning invoice`
- `Pay Lightning Address`
- `Scan QR`
- `Copy invoice`
- `Onchain address`
- `Seed phrase`
- `Mint`
- `Allowed mints`
- `Preferred mint`
- `Cashu payment request`

Avoid in primary UI unless the surface is technical: `melt`, `blind signature`, `proof`, `pending proofs`, `DLEQ`, `counter`, `keyset`, `NUT-XX`, and unexplained `fee reserve`.
