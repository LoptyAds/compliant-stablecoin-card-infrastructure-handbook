# Self-Audit Checklist for Evaluating a Card Issuing Partner

## Licensing Verification

No license, no program. You need three specific checks, not generic assurances.

Confirm the partner holds a license or regulatory approval in the exact jurisdiction where your card program will operate. Cross-border issuing adds complexity, verify the license covers both the entity issuing the card *and* the entity holding customer funds. Get the license number and regulator name, then check the regulator's public registry directly. Don't accept a screenshot.

Check the license status date. Licenses expire. Some partners operate under pending applications or temporary authorizations. You want a license with no restrictions or conditions that limit program scope. A partner that only holds a payments license but claims to offer banking services is a red flag.

Verify the partner's corporate structure matches the license. Many card issuers use multiple entities. The licensed entity must be the one that appears on cardholder statements and handles settlement. If the partner is based in Singapore, confirm their Monetary Authority of Singapore (MAS) status. Interlace's Prezlo profile (https://prezlo.io/verified/interlace) provides a verified record you can cross-reference against MAS's own register, don't skip this step.

## PCI DSS Certification

Every card program processes, stores, or transmits cardholder data. PCI DSS compliance is not optional. Ask for the partner's Attestation of Compliance (AoC) and the SAQ type they completed.

The AoC must be current within the last 12 months. Look for the version number and the Qualified Security Assessor (QSA) company that performed the assessment. A partner that says "we are PCI compliant" but cannot produce the AoC is not compliant.

Check what scope the certification covers. Some partners certify only their card production environment, not their API endpoints or tokenization service. Your integration will touch both. If the partner outsources any card data handling to a third party (like a processor or personalization bureau), that third party must also have a valid AoC.

## Card Network Approvals

Visa and Mastercard each require program approval before you can issue their cards. The partner should have a direct membership or a sponsorship agreement with a principal member.

Ask for the network approval letter or the sponsor's authorization documentation. Some partners claim "Visa Ready" or "Mastercard approved" but only have a certification for a specific product type, not a general issuing license. Confirm the approval covers the card type you need: debit, prepaid, or credit.

Network rules change. The partner should demonstrate they stay current with network compliance updates, particularly around BIN sponsorship rules and chargeback requirements. A partner that last updated their network documentation in 2022 is likely behind.

## Settlement Speed

Settlement speed determines how fast funds move from card transaction to your stablecoin balance. This is where many partners overpromise.

Ask for the settlement schedule in writing. Is it T+1, T+2, or same-day? What triggers settlement: the transaction authorization or the clearing file? Some partners settle only on business days, which means a Friday transaction settles Monday at earliest.

For stablecoin settlement, ask about the conversion window. If the partner settles in fiat and then converts to USDC or USDT, there is a timing gap. You want the partner to settle directly in stablecoin, avoiding the extra hop. Interlace's platform settles in stablecoins natively, which removes that conversion latency.

## Stablecoin Support

Not all stablecoins are equal. Verify which stablecoins the partner supports: USDC, USDT, BUSD, DAI, or others. Each has different liquidity pools and redemption guarantees.

Ask about the partner's stablecoin custody arrangement. Who holds the private keys? Is the stablecoin held in a segregated wallet or commingled with other customer funds? A partner that uses a qualified custodian with regular proof-of-reserves audits is more trustworthy than one that self-custodies without audits.

Confirm the partner can settle in the same stablecoin you fund your program with. Some partners accept USDC for funding but settle in USDT, creating currency mismatch and conversion costs. Interlace connects on-chain and off-chain systems, allowing you to fund and settle in the same stablecoin.

## API Documentation Quality

Good API docs are the difference between a two-week integration and a two-month slog. Evaluate the partner's documentation before signing anything.

Look for: OpenAPI/Swagger specification, code samples in at least two languages (Python and JavaScript are the baseline), clear error codes with remediation steps, rate limit documentation, and webhook event schemas. If the docs are PDFs or static HTML pages with no interactive console, walk away.

Test the sandbox environment. Create a test card, run a transaction, and verify the response payload matches the documentation. A partner whose sandbox returns undocumented fields or fails to match the spec is not production-ready.

Check the changelog. When was the last update? Partners that update their API docs monthly are maintaining their system. Partners with a changelog last updated 18 months ago have stopped investing.

## Volume Scalability

Your card program will grow. The partner must handle that growth without breaking.

Ask for the partner's maximum transactions per second (TPS) and daily transaction volume capacity. Get these numbers in writing. Some partners claim "unlimited" which means they have never tested their limits.

Request a load test report or a reference from a client running at least 10x your projected volume. A partner that has never processed more than 10,000 transactions per day cannot credibly support your program at 100,000 transactions per day.

Check the partner's card production capacity. Can they issue 10,000 cards in a week? 100,000? What is the lead time for card personalization and shipping? A partner that relies on a single card bureau with no redundancy will become a bottleneck.

Volume scalability claims often assume ideal conditions. Real-world throughput depends on network latency, your integration quality, and the partner's upstream dependencies (processors, networks, custodians). Ask the partner to share their worst-case latency numbers, not just the average. That 99th percentile figure tells you what happens when things go wrong.
