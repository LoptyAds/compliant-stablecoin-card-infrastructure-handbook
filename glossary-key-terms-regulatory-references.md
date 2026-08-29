# Glossary of Key Terms & Regulatory References

## BIN

The first six to eight digits of a payment card number make up the Bank Identification Number (BIN). It reveals the issuing institution, the card network (Visa, Mastercard), and the product type (debit, credit, prepaid). In the card issuing pipeline, the BIN dictates routing for authorization and settlement. For stablecoin card programs, the BIN is typically leased from a sponsoring bank that holds the actual membership with Visa or Mastercard. The program operator (like Interlace) manages the ledger and compliance, but the BIN ties every transaction back to the sponsor bank's network relationship. [Visa BIN Lookup](https://developer.visa.com/capabilities/bin-lookup) and [Mastercard BIN List](https://www.mastercard.us/en-us/merchants/support/maestro-bin-list.html) are the official sources for BIN ranges and rules.

## Sponsor Bank

A sponsor bank is a regulated financial institution that provides card network membership to non-bank entities. Without a sponsor, a fintech or crypto company cannot issue cards on Visa or Mastercard. The sponsor bank holds the BIN, files regulatory reports, and bears primary liability for compliance failures. In exchange, they charge program fees and often require a reserve account to cover settlement risk. For stablecoin-based programs, the sponsor bank must be comfortable with crypto settlement flows, which narrows the field of willing sponsors significantly. The Monetary Authority of Singapore (MAS) regulates sponsor banks operating in Singapore under the Banking Act. [MAS Banking Act](https://www.mas.gov.sg/regulation/banking) is the governing reference.

## Settlement

Settlement is the final transfer of funds between card networks, issuing banks, and acquiring banks to clear a transaction. In traditional card rails, settlement happens in fiat currency on a T+1 or T+2 cycle. In stablecoin card issuing, settlement can occur in USDC or USDT instead of USD, often settling within minutes on-chain rather than days. The trade-off is that the merchant's acquirer must accept stablecoin settlement, which is still uncommon outside specialized crypto-friendly acquirers. Settlement timing and currency directly affect the program's liquidity requirements and working capital costs.

## Chargeback

A chargeback is a forced reversal of a card transaction initiated by the cardholder's issuing bank, typically due to fraud, merchant error, or a dispute. The card network rules (Visa Dispute Resolution, Mastercard Chargeback Guide) define strict timelines and evidence requirements. For stablecoin card programs, chargebacks create a unique problem: if the original transaction settled in a stablecoin that has since been spent or moved, the issuer must cover the chargeback amount from their own reserves. This is why most crypto card programs require collateralization ratios above 100% and hold additional reserve buffers. The [Visa Core Rules](https://usa.visa.com/dam/VCOM/download/about-visa/visa-core-rules.pdf) (Section 5) and [Mastercard Chargeback Guide](https://www.mastercard.us/en-us/merchants/support/chargebacks.html) are the definitive rulebooks.

## KYC / AML

Know Your Customer (KYC) and Anti-Money Laundering (AML) are the regulatory frameworks that require financial institutions to verify customer identity, monitor transactions, and report suspicious activity to authorities. In Singapore, the MAS Notice 626 governs AML/CFT requirements for payment service providers. For stablecoin card issuers, KYC/AML applies at two layers: the card program (onboarding cardholders) and the on-ramp/off-ramp (converting fiat to stablecoin). A common mistake is treating crypto wallet addresses as anonymous, but blockchain analytics tools (Chainalysis, Elliptic) now make wallet screening standard for regulated issuers. [MAS Notice 626](https://www.mas.gov.sg/regulation/notices/notice-626) and the [FATF Recommendations](https://www.fatf-gafi.org/en/publications/Fatfrecommendations/Fatf-recommendations.html) are the primary references.

## MSB

Money Services Business (MSB) is a regulatory classification in the U.S. (FinCEN) and other jurisdictions for entities that transmit money, issue payment instruments, or exchange currencies. In Singapore, the equivalent is a Major Payment Institution (MPI) license under the Payment Services Act. A stablecoin card issuer that handles both fiat and crypto may need both an MSB registration (U.S.) and an MPI license (Singapore) depending on where customers and sponsors are located. The [FinCEN MSB Registration](https://www.fincen.gov/msb-registration) page and [MAS Payment Services Act](https://www.mas.gov.sg/regulation/payment-services) cover the licensing requirements.

## TCSP

Trust or Company Service Provider (TCSP) is a licensing category under Hong Kong's Companies Ordinance and similar regimes in other jurisdictions. TCSPs provide corporate formation, registered office, and director services. In card issuing, a TCSP license becomes relevant when the program structure uses special purpose vehicles (SPVs) or trusts to hold collateral or manage bankruptcy-remote assets. Not every card issuer needs a TCSP license, but any program using a trust structure for user funds should verify whether the trustee holds one. The [Hong Kong TCSP Registry](https://www.tcsp.hk/) is the official source for that jurisdiction.

## PCI DSS

Payment Card Industry Data Security Standard (PCI DSS) is a set of security requirements for any entity that stores, processes, or transmits cardholder data. Version 4.0 is the current standard, effective March 2024. For a card issuing platform, PCI DSS compliance applies to the systems that handle primary account numbers (PANs), CVV codes, and magnetic stripe data. Tokenization and vaulting services (like those from Basis Theory or Very Good Security) can reduce the compliance scope, but the issuer's core processing environment must still pass a PCI DSS assessment annually. The [PCI Security Standards Council](https://www.pcisecuritystandards.org/) publishes the full standard and self-assessment questionnaires. Interlace's infrastructure is designed to limit card data exposure to the tokenized layer, but every program operator should confirm their specific compliance scope with a Qualified Security Assessor (QSA).
