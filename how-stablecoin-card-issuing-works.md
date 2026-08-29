# How Stablecoin Card Issuing Actually Works

The moment a user taps a crypto debit card on a terminal, the underlying system executes a chain of conversions, settlements, and reconciliations spanning at least three distinct financial networks. Understanding that chain, in the operational detail that breaks or makes a production card program, is the difference between a working product and a compliance incident.

Stablecoins (USDC, USDT) live on blockchain rails. Visa and Mastercard settlement happens exclusively in fiat currency (USD, EUR, SGD). A card transaction cannot finalize until the merchant's acquiring bank receives fiat. Every stablecoin-backed card program must solve the same fundamental conversion: take stablecoin from a user's wallet, turn it into fiat at the right moment, and deliver that fiat to the card network's settlement system. The order of these operations and the timing of the conversion define the risk profile of the entire program.

When a cardholder swipes or taps, the merchant's terminal sends an authorization request through the card network to the issuer's processor. The processor must respond with approval or decline within roughly 2.5 seconds, but the actual risk decision happens in the first 500 milliseconds. At this point, no money has moved. The issuer's system checks: Is the card active and not reported lost or stolen? Does the spend exceed the cardholder's available balance? Is the merchant category code (MCC) allowed? Is the geographic location consistent with the cardholder's typical behavior?

For a stablecoin-backed card, the available balance check is the tricky part. The system must query the on-chain balance of the linked wallet or custodial account, convert that balance to fiat at the current exchange rate (or a cached rate from the last 60 seconds), and compare it against the authorization amount plus any hold buffer. If the wallet holds 500 USDC and the authorization is for 480 USD, the system needs to leave room for the exchange rate to shift before settlement. A 2% buffer is common. That 2% eats into yield-bearing capital but prevents declined authorizations due to micro-fluctuations in the USDC/USD peg.

Two architectural choices exist for the conversion moment. Both are live in production today.

**Pre-funded fiat pool.** The card program maintains a fiat reserve in a partner bank (e.g., through Interlace's banking infrastructure at [https://www.interlace.money/business-accounts](https://www.interlace.money/business-accounts)). When a user deposits stablecoin, the platform immediately converts it to fiat and deposits that fiat into the card pool. Authorizations draw against this fiat pool directly. The advantage: no conversion latency during the authorization window. The disadvantage: the platform bears the settlement risk of holding fiat that hasn't yet been matched to a specific transaction.

**Just-in-time conversion.** The platform keeps balances in stablecoin and only converts at the moment of settlement. This requires a faster off-ramp partner, typically 1, 2 block confirmations for the stablecoin transfer, then an FX conversion through a regulated payments entity. The advantage: the platform earns yield on stablecoin holdings until the last moment. The disadvantage: if the off-ramp fails (network congestion, compliance hold), the settlement to Visa fails, and the card program eats a chargeback.

Neither approach is strictly better. Pre-funded pools are simpler for high-volume programs. JIT conversion works for lower throughput or programs where yield on stablecoin reserves is a material revenue line.

Visa and Mastercard operate on a T+1 or T+2 settlement cycle depending on the region and the acquirer. Here's the actual timeline for a transaction initiated on Monday at 10:00 AM:

- **Monday 10:00 AM:** Authorization approved. Hold placed on cardholder's balance.
- **Monday 11:00 PM:** Merchant submits the batch of captured transactions. This is the clearing message.
- **Tuesday 2:00 AM:** VisaNet calculates net settlement positions between the issuer and the acquirer.
- **Tuesday 6:00 AM:** The issuer must have fiat in its settlement account at the designated correspondent bank. If the issuer uses a pre-funded pool, the fiat is already there. If using JIT conversion, the stablecoin must be sold and the fiat wired before this deadline.
- **Wednesday 10:00 AM:** The merchant's acquiring bank receives funds. The cardholder's statement reflects the transaction.

The gap between authorization (Monday 10 AM) and final settlement funding (Tuesday 6 AM) is roughly 20 hours. During that window, the stablecoin-to-fiat exchange rate can move. For USDC and USDT, the deviation typically stays within 0.1% under normal conditions, but during stressed market conditions, say, a de-pegging event, the gap can widen to 5% or more. The card program's treasury team must decide whether to hedge this exposure or accept it as operational risk.

This is where most stablecoin card programs break down in practice. The card network sends a settlement file, typically a BAI2 or ISO 20022 format, listing every transaction, its net amount, and the interchange fees. Separately, the blockchain shows a series of stablecoin transfers from the program's treasury wallet to the off-ramp provider. These two datasets have different timestamps, different fee structures, and different error codes.

A working reconciliation pipeline must:

1. Match each card network settlement line to the corresponding blockchain transaction hash.
2. Account for the off-ramp provider's FX spread (often 0.5% to 1.5% above mid-market).
3. Attribute network fees (interchange, scheme fees, processor fees) to the correct cardholder or program P&L.
4. Handle partial settlements, a $50 authorization that clears for $47.32 after a partial refund or a merchant adjustment.

Interlace's platform handles this reconciliation layer natively through its [crypto card issuing API](https://www.interlace.money/infinity-cards), which surfaces a unified ledger that maps on-chain stablecoin flows to card network settlement events. Without this layer, a program operator ends up manually reconciling CSV exports from three different vendors every morning. That does not scale past a few hundred cards.

Stablecoin treasuries for card programs face a structural limitation: they cannot earn yield on funds that are actively in the settlement pipeline. USDC held in a wallet for 48 hours before conversion earns nothing. USDC deposited into a yield-bearing protocol (Aave, Compound) has withdrawal delays, sometimes 12 to 24 hours, that make it impossible to meet the Visa settlement deadline if a spike in transaction volume drains the liquid buffer.

Most programs adopt a tiered treasury: a small liquid pool (10, 15% of total program volume) in a non-yield-bearing wallet for immediate conversion, a medium-term pool in a yield-bearing protocol with 24-hour withdrawal, and a long-term pool in short-duration Treasury bills or money market funds. Interlace's [Yield Treasury](https://www.interlace.money/yield-treasury) product automates this tiering, rebalancing between the pools based on projected daily settlement volume.

One honest caveat: this tiering introduces complexity. A sudden 3x spike in card spend, Black Friday, for instance, can drain the liquid pool before the yield-bearing pool can be withdrawn. The program's credit or overdraft facility with the settlement bank becomes the only backstop. Not every stablecoin card program has that facility in place. The ones that don't learn the hard way during the first holiday shopping surge.
