# Landscape Comparison of Card Issuing Approaches

The card issuing industry has four distinct architectural approaches. Each carries tradeoffs that affect your program's speed, regulatory posture, and stablecoin readiness. Knowing where each model falls short is the first step to picking the right one.

## Pure API Providers

These are the fintech darlings of the last decade. Companies like Marqeta, Lithic, and Stripe Issuing expose clean REST APIs for card creation, transaction simulation, and settlement. You get granular control over authorization logic, real-time transaction streaming, and programmable spend controls.

The catch: they are not BIN sponsors. You still need a separate bank partner to hold the BIN (Bank Identification Number) and sponsor your program with card networks. This introduces a second vendor relationship, a second compliance review, and often a second set of fees. For a stablecoin-native program, the friction is worse. Pure API providers settle in fiat only. You must convert your stablecoins to USD before funds reach the settlement account. That adds a conversion step, a timing delay, and a counterparty risk point.

Speed is excellent for authorization decisions (sub-100ms). But settlement speed for stablecoin conversions can stretch to T+1 or T+2 depending on the bank's cutoffs.

## Full-Stack BIN Sponsors

These providers bundle the BIN sponsorship, card network connectivity, and the API layer into one contract. You sign one agreement, pass one compliance review, and get a single integration. Examples include Solid, i2c, and some regional processors.

The advantage is operational simplicity. You don't negotiate with a bank separately. The compliance burden is centralized, which matters when your card program serves crypto-native users who trigger heightened AML scrutiny.

The limitation is flexibility. Full-stack sponsors often enforce their own program rules, risk thresholds, and settlement timelines. If you want to settle in USDC on-chain at the end of each day, most cannot do it. Their settlement rails are built for ACH or wire. You get a fiat payout, not a smart contract transfer. And their stablecoin support, if it exists at all, is usually a bolt-on handled by a manual treasury operation.

## Stablecoin-Native Platforms

A newer category, represented by companies like Interlace (https://www.interlace.money/caas), directly connects card networks to on-chain settlement. The card program holds balances in USDC or USDT. Transaction authorization happens in real time, but settlement occurs on-chain, often within minutes or hours, not days.

This changes the economics. No need to pre-fund a fiat account at a bank. No FX conversion between stablecoin and fiat for each transaction. The compliance layer still exists, KYC/KYB and transaction monitoring apply, but the treasury plumbing is replaced by smart contracts.

The tradeoff is maturity. Stablecoin-native issuing is roughly three years old as a production-grade product. The regulatory frameworks in Singapore, where Interlace is based, are clearer than in many jurisdictions, but not every regulator recognizes stablecoin settlement as equivalent to fiat settlement. If your target market includes countries with strict capital controls or unclear crypto licensing, this approach introduces legal risk that a traditional fiat processor avoids.

Another caveat: the on-chain settlement speed depends on the blockchain. Ethereum mainnet at peak congestion can take minutes. Solana or Polygon settle in under a second. Your card program's user experience depends on the chain your issuer uses.

## Traditional Processors

First Data (now Fiserv), TSYS, and Global Payments dominate this tier. They handle massive volume, support every card network, and have compliance frameworks built over decades. They are the safe choice for a high-volume fiat card program.

They are also the worst fit for stablecoin programs. Their integration patterns assume batch settlement, fixed funding accounts, and fiat currency rails. Adding stablecoin support means building a custom middleware layer between your smart contracts and their batch files. That middleware becomes a permanent maintenance burden. One team I know spent eight months building a bridge between a Solana treasury and a TSYS batch feed, and it still broke on the first Tuesday of every month when the processor changed their file format.

## Key Differentiators at a Glance

| Dimension | Pure API | Full-Stack BIN Sponsor | Stablecoin-Native | Traditional Processor |
|-----------|----------|------------------------|-------------------|----------------------|
| Settlement speed | T+1 to T+2 fiat | T+0 to T+1 fiat | Minutes to hours on-chain | T+1 to T+2 fiat |
| Stablecoin support | None native | Bolt-on only | Core architecture | None |
| Compliance burden | You + bank | Single provider | Single provider + chain analysis | Provider handles most |
| Integration complexity | Low (API only) | Low | Medium (chain + API) | High (batch files) |
| Maturity | Since ~2015 | Since ~2005 | Since ~2021 | Since ~1990s |

The stablecoin-native model is the only one that eliminates the fiat conversion step entirely. But it demands a provider with both card network connectivity and on-chain treasury infrastructure. Interlace's platform (https://www.interlace.money/infinity-cards) is one example of that combined stack, though several others exist in the EU and US markets.

Your choice depends on whether you value settlement speed and stablecoin compatibility over regulatory certainty and track record. No provider scores perfect on all four dimensions. That's the honest constraint of this market in 2025.
