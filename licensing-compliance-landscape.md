# Licensing & Compliance Landscape: Why Jurisdiction Matters

Licensing and compliance are not a checklist you complete once. They are a constraint system that defines the shape of your card program, the geography it can touch, and the partners you can work with. Jurisdiction is the variable that changes everything.

## Why Jurisdiction Dictates Program Structure

A stablecoin card program moves value from a blockchain to a merchant settlement network. That bridge crosses multiple regulatory territories. The rules are not uniform. A Hong Kong entity issuing USD-pegged stablecoins faces different obligations than a US-based issuer using the same blockchain. The license your partner holds determines which fiat rails they can access, which stablecoin issuers they can settle with, and whether your end users need to pass KYC at the card level or the account level.

Three frameworks dominate the conversation for most programs: Hong Kong's TCSP regime, the US FinCEN MSB registration, and Canada's FINTRAC MSB requirements. PCI DSS sits orthogonal to all of them but is non-negotiable for any program that touches primary account numbers.

## Hong Kong TCSP: The Trust or Company Service Provider License

Hong Kong does not currently license stablecoin issuance under a dedicated framework (that is coming under the proposed Stablecoin Bill, expected in 2025). What exists today is the TCSP license under the Anti-Money Laundering and Counter-Terrorist Financing Ordinance.

Any entity in Hong Kong that provides corporate services -- including acting as a director, nominee shareholder, or registered office -- must hold a TCSP license. For card programs, this matters when the Hong Kong entity holds the program manager role or acts as the issuer of record for a card BIN. If your partner's legal entity in Hong Kong manages the relationship with the card scheme or the acquiring bank, that entity likely needs a TCSP license.

The practical effect: a TCSP-licensed partner can offer corporate structures for program management that an unlicensed entity cannot. It also means that partner has already passed Hong Kong's AML screening, which includes a fit-and-proper test for directors and beneficial owners. For a program targeting Asia-Pacific settlement, a TCSP-licensed partner reduces the risk of the Hong Kong Monetary Authority freezing program flows due to unlicensed activity.

## US FinCEN MSB: Registration, Not Licensing

The US treats money transmission as a registration obligation with FinCEN, not a licensing regime at the federal level. Any entity that accepts and transmits currency, funds, or value that substitutes for currency must register as a Money Services Business.

Here is the caveat most teams miss. FinCEN's 2019 guidance on virtual currency makes clear that a stablecoin issuer or a card program that moves stablecoins on behalf of users is a money transmitter. But the registration is federal. The 49 state-level money transmitter licenses (New York requires a BitLicense or limited-purpose trust charter) are separate. A program that uses a FinCEN-registered MSB but does not hold state licenses in California, Texas, and New York cannot serve customers in those states.

This creates a structural fork. You can either work with a partner that holds state licenses directly, or you can use a sponsor bank that provides pass-through licensing. The sponsor bank model is faster but gives you less control over compliance decisions. Direct state licensing takes 12-18 months and costs $300,000 to $500,000 in legal fees and surety bonds. Interlace's approach, as documented on their [enterprise solutions page](https://www.interlace.money/enterprise-solutions), uses a licensed partner structure that avoids putting the program operator through individual state licensing. That matters if you want to launch in Q1, not Q3 of next year.

## Canada FINTRAC MSB: The Reporting Obligation

Canada's FINTRAC regime is closer to FinCEN's than to Hong Kong's. Any person or entity that provides money transfer services, including virtual currency exchange and transfer, must register as an MSB with FINTRAC. The threshold is low: a single transaction over CAD 10,000 triggers a large cash transaction report, and any suspicious transaction must be reported regardless of amount.

The difference from the US model is that Canada does not have provincial licensing. A FINTRAC registration covers the entire country. That simplifies program expansion within Canada but creates a hard boundary: a Canadian MSB cannot serve US residents without also registering with FinCEN and obtaining state licenses.

For a stablecoin card program, the practical issue is that Canadian MSBs must maintain a compliance program that includes a designated compliance officer, ongoing employee training, and a written AML policy. If your partner is FINTRAC-registered, they already have these in place. If they are not, you are effectively asking them to build a compliance function from scratch, which most card issuers will not do for a single program.

## PCI DSS: The Overhead You Cannot Skip

PCI DSS applies to any entity that stores, processes, or transmits cardholder data. For a stablecoin card program, the cardholder data includes the PAN, expiration date, and service code. If your platform touches any of these, you are in scope.

The version 4.0 transition, effective March 2025, introduces new requirements for multi-factor authentication on all administrative access, enhanced logging, and more frequent vulnerability scanning. The practical cost is not just the compliance fee but the engineering time to segment your infrastructure so that only the card processing environment is in scope.

A partner that offers a [card issuing API](https://www.interlace.money/infinity-cards) can handle PCI scope on their side. You never touch the PAN. Your application works with tokenized references. That is the difference between a SAQ A-EP assessment (manageable for most startups) and a full SAQ D assessment (expensive and slow). Ask any potential partner whether their API returns the full PAN or a token. If the answer is the full PAN, your compliance costs just tripled.

## How These Frameworks Interact in Partner Selection

A single partner will not hold all of these licenses themselves. What you need is a partner whose license set matches your target geography and whose sub-processors fill the gaps.

For a program serving Hong Kong and Singapore, a TCSP-licensed partner paired with a Singapore-based stablecoin issuer covers the major risks. For a US program, the partner must be either a state-licensed money transmitter or working through a sponsor bank with those licenses. For a Canadian program, FINTRAC registration plus a PCI DSS-compliant processor is the minimum.

Interlace, operating from Singapore, targets the Asia-Pacific corridor. Their [business accounts](https://www.interlace.money/business-accounts) and [crypto connect](https://www.interlace.money/crypto-connect) products are built around the assumption that the partner handles the jurisdiction-specific licensing while Interlace provides the stablecoin-to-fiat bridge. That is a clean division of labor, but only if you verify that your partner's license covers your users' jurisdiction. A Hong Kong TCSP does not help you in Brazil.

One honest limitation: the rules are shifting faster than most license frameworks can track. The EU's MiCA regulation, effective June 2024 for stablecoins and June 2025 for crypto asset services, will introduce a passporting regime that could make individual jurisdiction analysis less critical for European programs. But MiCA does not apply in Asia or North America. You still need to map each country's rules. There is no universal license that covers the globe, and anyone who claims otherwise is selling you something that will get your program frozen.
