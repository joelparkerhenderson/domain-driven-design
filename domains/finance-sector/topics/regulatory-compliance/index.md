# Regulatory Compliance in Finance

## Overview

Regulatory compliance encompasses the legal and governance requirements that shape domain model design in financial systems. Financial institutions operate under extensive regulatory frameworks that dictate how they manage capital, report transactions, protect customer data, prevent financial crime, and ensure market integrity. These requirements are not optional add-ons -- they are first-class domain concerns that must be modeled explicitly.

## Relevance to Finance

Regulatory requirements fundamentally shape the design of every bounded context. Capital adequacy rules affect how risk is modeled. Anti-money laundering laws dictate customer onboarding workflows. Transaction reporting requirements influence event design. Privacy regulations constrain how customer data flows between contexts. Compliance is not an afterthought -- it is woven into the domain model from the start.

## Major Regulatory Frameworks

### Basel III / Basel IV

International regulatory framework establishing minimum capital requirements, liquidity standards, and leverage ratios for banks.

- **Capital adequacy**: Banks must maintain minimum ratios of capital to risk-weighted assets (CET1 >= 4.5%, Tier 1 >= 6%, Total >= 8%)
- **Liquidity coverage ratio (LCR)**: Banks must hold sufficient high-quality liquid assets to cover 30 days of net cash outflows
- **Net stable funding ratio (NSFR)**: Requires stable funding relative to the liquidity profile of assets
- **Leverage ratio**: Non-risk-based backstop to the risk-weighted capital framework
- **DDD impact**: The Risk & Compliance context must calculate risk-weighted assets, capital ratios, and liquidity metrics. RiskAssessment aggregates must track exposure classifications and risk weights.
- **Basel IV refinements**: Revised standardized approaches for credit risk, operational risk, and output floors limiting the benefit of internal models

### Sarbanes-Oxley Act (SOX)

U.S. federal law requiring financial reporting controls and management accountability.

- **Section 302**: CEO and CFO certification of financial report accuracy
- **Section 404**: Assessment of internal controls over financial reporting (ICFR)
- **Section 409**: Real-time disclosure of material changes in financial condition
- **DDD impact**: The Accounts & Ledger context must enforce segregation of duties, maintain complete audit trails, and support financial statement certification processes. Immutable journal entries and event sourcing naturally support SOX audit requirements.

### Dodd-Frank Wall Street Reform Act

Comprehensive U.S. financial regulation enacted after the 2008 financial crisis.

- **Volcker Rule**: Restricts proprietary trading by banks
- **Derivatives reform**: Mandatory clearing and reporting of OTC derivatives through swap execution facilities
- **Consumer protection**: Established the Consumer Financial Protection Bureau (CFPB)
- **Stress testing**: Annual Comprehensive Capital Analysis and Review (CCAR) for large banks
- **DDD impact**: The Trading & Investments context must enforce trading restrictions. The Risk & Compliance context must support stress testing scenarios and regulatory reporting.

### PSD2 (Payment Services Directive 2)

European regulation opening payment services to competition and mandating strong customer authentication.

- **Strong Customer Authentication (SCA)**: Multi-factor authentication required for electronic payments
- **Open Banking**: Third-party providers (TPPs) can access customer account data with consent (AISP) or initiate payments on behalf of customers (PISP)
- **API standards**: Banks must provide APIs for third-party access (Berlin Group, Open Banking UK)
- **DDD impact**: The Payments & Transfers context must support third-party access and SCA flows. The Customer Onboarding context must manage consent for data sharing.

### GDPR (General Data Protection Regulation)

European regulation governing the processing and protection of personal data.

- **Lawful basis**: Processing must have a lawful basis (consent, contract, legal obligation, legitimate interest)
- **Data minimization**: Collect only the data necessary for the stated purpose
- **Right to erasure**: Individuals can request deletion of their personal data (balanced against financial record retention requirements)
- **Data portability**: Individuals can request their data in a machine-readable format
- **Breach notification**: Authorities must be notified within 72 hours of a personal data breach
- **DDD impact**: All bounded contexts handling personal data must implement data protection by design. Value objects containing personal data must support pseudonymization. Event stores must handle erasure requests (crypto-shredding).

### KYC/AML (Bank Secrecy Act)

U.S. anti-money laundering framework requiring financial institutions to detect and report financial crime.

- **Customer Identification Program (CIP)**: Verify customer identity at account opening
- **Customer Due Diligence (CDD)**: Understand the nature of the customer relationship and expected activity
- **Enhanced Due Diligence (EDD)**: Additional scrutiny for high-risk customers
- **Suspicious Activity Reports (SARs)**: File reports with FinCEN for transactions suspected of involving money laundering or terrorist financing
- **Currency Transaction Reports (CTRs)**: Report cash transactions exceeding $10,000
- **Beneficial ownership**: Identify and verify individuals who own 25% or more of a legal entity
- **DDD impact**: The Customer Onboarding context must implement CIP/CDD/EDD workflows. The Risk & Compliance context must monitor transactions and generate SARs/CTRs. The Payments & Transfers context must screen transactions against sanctions lists.

### MiFID II (Markets in Financial Instruments Directive II)

European regulation governing securities markets, investor protection, and market transparency.

- **Best execution**: Firms must take sufficient steps to obtain the best possible result for clients
- **Transaction reporting**: Report all transactions in financial instruments to regulators (within T+1)
- **Pre- and post-trade transparency**: Publish quotes and trade data for regulated instruments
- **Product governance**: Manufacturers and distributors must ensure products meet target market needs
- **Research unbundling**: Separate research costs from trading commissions
- **DDD impact**: The Trading & Investments context must capture execution quality data, report transactions to regulators, and enforce best execution policies. The Risk & Compliance context manages transaction reporting.

### FATCA (Foreign Account Tax Compliance Act)

U.S. law requiring foreign financial institutions to report information about accounts held by U.S. taxpayers.

- **Reporting**: Foreign financial institutions report account balances, interest, dividends, and gross proceeds to the IRS
- **Withholding**: 30% withholding tax on certain U.S.-source payments to non-compliant institutions
- **Intergovernmental Agreements (IGAs)**: Bilateral agreements facilitating FATCA compliance
- **DDD impact**: The Customer Onboarding context must capture U.S. tax status during onboarding. The Accounts & Ledger context must support FATCA reporting requirements. The Risk & Compliance context manages annual FATCA filings.

## Cross-Cutting Compliance Concerns

### Audit Trails

All bounded contexts must maintain complete, tamper-evident records of who did what and when. Event sourcing naturally provides this capability.

### Data Retention

Financial regulations mandate retention of transaction records, communications, and customer data for specified periods (typically 5-7 years). This must be balanced against GDPR erasure rights.

### Segregation of Duties

Critical operations (e.g., creating and approving journal entries, initiating and authorizing payments) must require different individuals, enforced by the domain model.

### Regulatory Reporting Calendar

Many regulations require periodic filings: daily transaction reports, quarterly capital adequacy returns, annual FATCA filings. The domain must track reporting obligations and deadlines.

## Relationships to Other Topics

- [Risk & Compliance Context](../risk-compliance-context/) -- The primary bounded context for managing regulatory obligations
- [Customer Onboarding Context](../customer-onboarding-context/) -- KYC/AML and FATCA requirements shape customer onboarding
- [Accounts & Ledger Context](../accounts-ledger-context/) -- SOX and financial reporting regulations shape ledger design
- [Payments & Transfers Context](../payments-transfers-context/) -- PSD2 and sanctions screening requirements shape payment processing
- [Trading & Investments Context](../trading-investments-context/) -- MiFID II and Dodd-Frank shape trading operations
- [Finance Standards](../finance-standards/) -- XBRL, ISO 20022 are the technical formats for regulatory submissions
- [Event-Driven Architecture](../event-driven-architecture/) -- Event sourcing supports audit trail and compliance requirements

## References

- Basel Committee on Banking Supervision. "Basel III: International Regulatory Framework for Banks." BIS, 2017.
- U.S. Congress. "Sarbanes-Oxley Act of 2002." Public Law 107-204.
- U.S. Congress. "Dodd-Frank Wall Street Reform and Consumer Protection Act." Public Law 111-203, 2010.
- European Parliament. "Directive (EU) 2015/2366 (PSD2)." Official Journal of the European Union, 2015.
- European Parliament. "Regulation (EU) 2016/679 (GDPR)." Official Journal of the European Union, 2016.
- Financial Crimes Enforcement Network. "Bank Secrecy Act." https://www.fincen.gov/
- European Parliament. "Directive 2014/65/EU (MiFID II)." Official Journal of the European Union, 2014.
- U.S. Congress. "Foreign Account Tax Compliance Act (FATCA)." 26 U.S.C. Sections 1471-1474, 2010.
- FATF. "International Standards on Combating Money Laundering." https://www.fatf-gafi.org/
