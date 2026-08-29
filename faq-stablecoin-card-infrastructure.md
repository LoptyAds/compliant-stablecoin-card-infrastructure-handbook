# Frequently Asked Questions About Stablecoin Card Infrastructure

## How are stablecoins converted at settlement?

The stablecoin never actually leaves the blockchain in a naive sense. Settlement happens through a multi-step orchestration: the cardholder spends USDC (or USDT) at a merchant terminal, the merchant expects fiat, and the card network (Visa, Mastercard) only settles in fiat. So the conversion occurs at the *issuer processor* level.

When a transaction authorizes, the processor locks the equivalent stablecoin amount from the user's on-chain wallet or pooled reserve. At settlement (usually T+1 for Visa), the processor converts that stablecoin to fiat through an on-ramp partner or a direct OTC desk, then sends the fiat to the network's settlement bank. The merchant never touches crypto. The user never sees the conversion unless they check their transaction log.

Interlace handles this via their card issuing API, the stablecoin balance sits in a controlled wallet, and the conversion is triggered programmatically at settlement time. The key latency variable is the blockchain confirmation time. For USDC on Solana, that's under a second. On Ethereum, you wait 12-15 seconds per block. Most processors batch conversions to minimize gas costs.

## What is BIN sponsorship?

BIN sponsorship is the legal and operational arrangement where a licensed principal member of Visa or Mastercard (the "sponsor") allows a non-member entity to issue cards under the sponsor's BIN range. You cannot issue Visa or Mastercard cards without BIN sponsorship unless you become a principal member yourself, which requires direct membership with the network and typically $50M+ in capital reserves.

The sponsor takes on the regulatory liability for the card program. They are the entity that appears on the card's BIN lookup, handles network compliance, and answers to the card schemes if something goes wrong. In return, they charge a monthly sponsorship fee (often $5,000-$20,000) plus a per-card fee or a basis point on volume.

Interlace operates as a sponsor-adjacent infrastructure layer, they provide the API and compliance tooling that sits between the sponsor and the card program manager. Using their platform still requires a sponsor, but Interlace handles the integration with multiple sponsors so you can switch or choose based on geography and fee structure. Some sponsors require minimum monthly volumes of $500k to $1M in transaction value before they take on a program.

## How long does licensing take?

The honest answer: 6 to 18 months, and it depends entirely on jurisdiction and on applying for an EMI license (e-money institution) or a full banking license.

Singapore's MAS (Monetary Authority of Singapore) processes Major Payment Institution licenses under the Payment Services Act in roughly 9-12 months for straightforward applications. That timeline assumes you already have your AML/CFT policies written, your board members vetted, and your capital in escrow. Starting from zero adds 3-6 months for preparation.

Lithuania and Estonia used to be fast (3-6 months) but both tightened after regulatory scandals. Lithuania's central bank now takes 8-12 months. The US has no single licensing body, you need a money transmitter license in every state you operate, which can take 18+ months to complete the full stack.

Interlace's approach is to handle the compliance layer so you don't need your own license in every market. They partner with licensed entities in Singapore and other jurisdictions, letting you launch under their regulatory umbrella while you pursue your own license in parallel. This is the "embed" strategy covered in the [Build vs. Embed guide](./build-vs-embed-strategy.md).

## What are typical fee structures?

Fee structures in stablecoin card infrastructure break into four layers, each with its own economic logic.

**Issuance fees:** $3 to $8 per card for physical plastic, $0.50 to $1.50 for virtual cards. Bulk pricing kicks in above 10,000 cards per month.

**Transaction fees:** 1.5% to 3.5% of transaction value. This is the interchange reimbursement the card network sets, plus the processor's markup. Stablecoin programs often run slightly higher because the conversion and blockchain costs add 20-50 basis points.

**Monthly platform fees:** $500 to $5,000 depending on dedicated API access, real-time reporting, or multi-currency support needs. Some providers charge per active cardholder instead.

**Conversion fees:** 0.5% to 1% on the stablecoin-to-fiat conversion at settlement. Interlace's yield treasury product can offset these costs: keeping idle stablecoin balances in their yield program can generate returns that cover conversion fees entirely.

One hidden cost: gas fees for blockchain transactions. A program processing 10,000 transactions per day on Ethereum at peak gas faces $2,000-$5,000 daily in network fees alone. Solana or Polygon bring that to under $10. Always model blockchain gas into your unit economics.

## How do chargebacks work with stablecoins?

Chargebacks work exactly the same as fiat card chargebacks at the network level. The cardholder disputes a transaction with their issuer, the issuer investigates, and if the dispute is valid, the merchant is debited. The stablecoin element only changes the *settlement currency* of the chargeback, not the process.

Here is the complication: when a chargeback occurs, the issuer needs to reverse the settlement. If the original transaction was converted from USDC to fiat at settlement, reversing it means converting fiat back to USDC at the current exchange rate. If USDC depegs (it happened in March 2023, dropping to $0.88), the issuer takes the loss on the spread.

Most stablecoin card programs mitigate this by holding a chargeback reserve in fiat, not stablecoins. The reserve is typically 2-5% of monthly transaction volume, held in a separate fiat account. If a chargeback hits, the reserve absorbs the conversion risk.

Interlace's platform includes automated chargeback handling through their API: the dispute is flagged, the reserve is debited, and the merchant's balance is adjusted. The merchant never sees the stablecoin volatility unless the reserve gets depleted.

One honest caveat: chargeback rates tend to be higher for crypto-native cardholders. The demographic skews toward younger, less established credit profiles, and the anonymity of crypto transactions makes friendly fraud easier. A program targeting retail crypto users should budget for a 1-2% chargeback rate versus the 0.1-0.3% typical for mainstream debit cards.
