# Build vs. Embed: Evaluating Your Card Program Strategy

The decision to build or buy card issuing infrastructure is not a product decision. It is a capital allocation decision with a multi-year lock-in period. For a fintech targeting stablecoin flows, the wrong choice can burn 18 months and $2M+ before a single card is in a wallet.

## The Real Cost of Building In-House

Building a card issuing program from scratch means becoming a payment institution. In Singapore, that means applying for a Major Payment Institution (MPI) license under the Payment Services Act. The timeline for a new MPI license application from a standing start is 12 to 18 months, assuming no material deficiencies in the application. You need a compliance officer with at least five years of relevant experience, an AML/CFT framework that passes MAS scrutiny, and minimum base capital of S$250,000 for a card issuance license. That is before you touch any code.

On the engineering side, you need to integrate with a card scheme network (Visa or Mastercard), a processor (Fiserv, TSYS, or Marqeta), a BIN sponsor, and a settlement bank. Each integration has its own certification process. Visa’s VTS certification alone typically requires 6 to 8 weeks of testing. The full stack from signing a BIN sponsorship agreement to issuing the first live card usually lands between 9 and 14 months for a well-funded team that has done it before. For a first-time team, add 40% to that timeline.

The operational burden is worse. Card programs require daily reconciliation of settlement files, chargeback handling within strict scheme timelines, and ongoing compliance with card scheme data security standards (PCI DSS Level 1 certification). That certification costs between $50,000 and $150,000 per year for a small program, plus the engineering cost of maintaining a compliant environment.

## What Embedding Actually Means

Embedding through a provider like Interlace compresses that timeline to weeks, not years. The provider holds the MPI license, the BIN sponsorship, the processor relationship, and the PCI-certified infrastructure. Your integration surface is an API that handles card creation, balance management, transaction authorization, and settlement in stablecoins. The compliance burden shifts to the provider for the core licensed activities, while you retain responsibility for your own KYC/KYB on your end users.

There is a tradeoff. You do not control the BIN, the processor contract, or the scheme relationship. If the provider changes processors or switches BIN sponsors, your cards may need to be reissued. You also accept the provider’s compliance stance on transaction monitoring and sanctions screening. If their risk appetite is narrower than yours, you will lose transactions you might have approved with your own controls.

## The Numbers That Matter

For a program targeting 10,000 active cards within the first year, the build-versus-embed comparison looks like this:

**Build in-house:** $800,000 to $1.5M in first-year legal, licensing, and compliance costs. 14 to 20 months to first card. Ongoing annual compliance and PCI costs of $120,000 to $200,000. Engineering team of 4 to 6 people dedicated to card infrastructure.

**Embed with Interlace:** $0 in licensing costs. 4 to 8 weeks to first card. Per-card or per-transaction pricing that scales with volume. Engineering team of 1 to 2 people for API integration.

The breakeven point where building becomes cheaper than embedding is somewhere around 100,000 to 200,000 active cards with high transaction volumes. Below that, the fixed costs of licensing, certification, and compliance eat the margin.

## The Stablecoin Specific

Stablecoin card issuing adds a layer of complexity that pure fiat programs do not have. Settlement in USDC or USDT requires on-chain liquidity management, custody relationships with qualified custodians, and real-time conversion between stablecoin and fiat at the point of transaction. Building that conversion layer yourself means integrating with a fiat on-ramp, managing a settlement account at a commercial bank, and handling the timing mismatch between blockchain confirmation times and card scheme settlement windows.

Interlace handles that conversion inside their infrastructure. The API accepts stablecoin balances, converts to fiat at the processor level, and settles the transaction. Your application never touches the fiat rails directly. That removes a class of operational risk around bank account management and reconciliation timing.

## One Honest Caveat

Embedding does not eliminate compliance work. It shifts the licensed activities to the provider, but you still need your own AML program for your users, your own data protection compliance under Singapore’s PDPA, and your own fraud detection on the application layer. Providers like Interlace handle the card-level compliance, not the business-level compliance. Fintech founders sometimes treat embedding as a magic compliance wand. It is not. It is a division of labor, not a delegation of all responsibility.

## When to Build

Build if your core differentiator is control over the card program economics, if you expect to issue more than 200,000 cards, or if you need to support custom authorization logic that the provider’s API cannot express. Build if you have the capital to wait 18 months for revenue.

## When to Embed

Embed if you need to ship a card product in the next quarter, if your core differentiator is the user experience around stablecoins rather than the card infrastructure itself, or if you want to test product-market fit before committing to a multi-million dollar licensing process. Embed if your engineering team is 10 people or fewer and you cannot afford 4 of them on card infrastructure.

The right answer depends on your timeline, your capital position, and your tolerance for regulatory process. But the worst answer is deciding you will build, then running out of cash during the license application. That happens more often than the industry likes to admit.

For a deeper look at how the stablecoin-to-fiat conversion actually works under the hood, see [How Stablecoin Card Issuing Actually Works](./how-stablecoin-card-issuing-works.md). For the regulatory details that affect both paths, see [Licensing & Compliance by Jurisdiction](./licensing-compliance-market.md).
