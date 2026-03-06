# Risk & Compliance Context

## Overview

The Risk & Compliance context evaluates and monitors financial risks and regulatory obligations across the institution. It manages credit risk, market risk, operational risk, liquidity risk, KYC/AML screening, fraud detection, and regulatory reporting. This context provides the risk assessment and compliance verification services that other contexts depend on before executing financial operations.

## Core Concepts

### Credit Risk

The risk that a borrower or counterparty will fail to meet their financial obligations. Credit risk management includes:

- **Probability of default (PD)**: Likelihood that a borrower will default within a given time horizon
- **Loss given default (LGD)**: Expected loss as a percentage of exposure if default occurs
- **Exposure at default (EAD)**: Total amount at risk at the time of default
- **Expected loss**: PD x LGD x EAD -- the anticipated loss on a portfolio
- **Credit concentration**: Risk from overexposure to a single borrower, industry, or geography

### Market Risk

The risk of losses from adverse changes in market prices:

- **Value at Risk (VaR)**: Statistical estimate of maximum potential loss over a defined period at a given confidence level
- **Stress testing**: Evaluating portfolio performance under extreme but plausible market scenarios
- **Sensitivity analysis**: Measuring how positions react to changes in market factors (interest rates, FX rates, equity prices)

### Operational Risk

The risk of loss from inadequate or failed internal processes, people, systems, or external events:

- **System failures**: Technology outages, data corruption, cyber attacks
- **Process failures**: Unauthorized trading, settlement errors, misrouted payments
- **External events**: Regulatory changes, natural disasters, third-party failures

### KYC/AML (Know Your Customer / Anti-Money Laundering)

Regulatory requirements to verify customer identity and monitor for financial crime:

- **Customer identification program (CIP)**: Verifying identity at account opening
- **Customer due diligence (CDD)**: Understanding the customer's business and expected activity
- **Enhanced due diligence (EDD)**: Additional scrutiny for high-risk customers (PEPs, high-risk jurisdictions)
- **Transaction monitoring**: Detecting suspicious patterns indicating money laundering, terrorist financing, or sanctions evasion
- **Suspicious Activity Reports (SARs)**: Filing reports with FinCEN for suspicious activity

### Fraud Detection

Real-time identification of potentially fraudulent transactions:

- **Rule-based detection**: Predefined rules for known fraud patterns (velocity checks, geographic anomalies)
- **Machine learning models**: Anomaly detection models trained on historical fraud data
- **Case management**: Investigation workflows for reviewing and resolving fraud alerts

## Aggregates in This Context

### RiskAssessment Aggregate

- **Root**: RiskAssessment (AssessmentID)
- **Contains**: RiskFactor, MitigationAction
- **Value objects**: RiskScore, RiskCategory, ExposureAmount
- **Key invariant**: Assessment must be completed before it can be used for decisions

### KYCProfile Aggregate

- **Root**: KYCProfile (ProfileID)
- **Contains**: IdentityVerification, ScreeningResult, DueDiligenceRecord
- **Value objects**: RiskRating, ScreeningStatus
- **Key invariant**: Profile must pass all required screening checks before customer activation

### FraudAlert Aggregate

- **Root**: FraudAlert (AlertID)
- **Contains**: AlertEvidence, InvestigationNote, Resolution
- **Value objects**: AlertSeverity, AlertCategory
- **Key invariant**: Alerts must be investigated and resolved within regulatory timeframes

## Domain Events

- RiskAssessmentCompleted -- assessment finalized with score and recommendation
- RiskLimitBreached -- exposure or position limit exceeded; triggers trading restrictions
- SuspiciousActivityDetected -- transaction flagged for potential fraud or money laundering
- SanctionsHitDetected -- party matched against sanctions or watchlist
- KYCVerificationCompleted -- customer identity verification completed
- RegulatoryReportSubmitted -- required regulatory filing completed

## Integration with Other Contexts

- **Payments & Transfers** (downstream): This context provides fraud screening and sanctions checking services that payments must pass before authorization
- **Trading & Investments** (downstream): Pre-trade risk checks enforce position limits and margin requirements
- **Lending & Credit** (partner): Credit risk assessments inform underwriting decisions; default events update risk models
- **Customer Onboarding** (downstream): KYC verification services gate customer activation

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- One of the six bounded contexts in the finance domain
- [Context Map](../context-map/) -- Upstream relationship to Payments and Trading; partnership with Lending
- [Domain Events](../domain-events/) -- Risk events trigger actions across multiple contexts
- [Regulatory Compliance](../regulatory-compliance/) -- Basel III/IV, KYC/AML (BSA), SOX, and Dodd-Frank requirements drive this context's design
- [Subdomains](../subdomains/) -- Fraud detection and risk modeling are core subdomains for many institutions
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between external sanctions lists, credit bureaus, and internal models

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Basel Committee on Banking Supervision. "Basel III: International Regulatory Framework for Banks." BIS, 2017.
- Financial Crimes Enforcement Network. "Bank Secrecy Act." https://www.fincen.gov/
- Saunders, Anthony & Cornett, Marcia. "Financial Institutions Management: A Risk Management Approach." McGraw-Hill, 2018.
- Hull, John C. "Risk Management and Financial Institutions." Wiley, 2018.
