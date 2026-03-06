# Customer Onboarding Context

## Overview

The Customer Onboarding context manages the process of establishing new customer relationships with the financial institution. It handles the application intake, identity verification, Know Your Customer (KYC) due diligence, risk classification, account opening, product enrollment, and document management. This context is the gateway through which all new customers enter the institution's ecosystem.

## Core Concepts

### Application Intake

The process of collecting customer information for a new relationship:

- **Individual customers**: Name, date of birth, address, tax identification number, employment information
- **Business customers**: Legal entity name, registration number, beneficial ownership structure, authorized signatories
- **Product selection**: The accounts, services, or products the customer wishes to open

### Identity Verification

Confirming that the applicant is who they claim to be:

- **Document verification**: Government-issued photo ID (passport, driver's license), proof of address
- **Electronic verification**: Cross-referencing against authoritative databases (e.g., credit bureaus, government registries)
- **Biometric verification**: Facial recognition, fingerprint matching for digital onboarding
- **Liveness detection**: Confirming the applicant is physically present (not a photograph or deepfake)

### Know Your Customer (KYC)

Regulatory requirement to understand the customer's identity, business, and expected account activity:

- **Customer Identification Program (CIP)**: Minimum identification requirements at account opening
- **Customer Due Diligence (CDD)**: Understanding the nature and purpose of the customer relationship
- **Enhanced Due Diligence (EDD)**: Additional scrutiny for high-risk customers such as Politically Exposed Persons (PEPs), customers from high-risk jurisdictions, or complex corporate structures
- **Beneficial ownership**: Identifying natural persons who ultimately own or control a legal entity (25% ownership threshold under FinCEN rules)

### Risk Classification

Assigning a risk rating to each customer based on their profile:

- **Low risk**: Domestic individual customers with straightforward banking needs
- **Medium risk**: Small businesses, customers with moderate transaction volumes
- **High risk**: PEPs, businesses in high-risk industries (casinos, money services), customers from sanctioned or high-risk jurisdictions
- **Prohibited**: Customers who cannot be accepted due to sanctions, regulatory prohibitions, or institutional policy

### Product Enrollment

Activating the customer's selected products and services:

- **Deposit accounts**: Checking, savings, certificates of deposit
- **Credit products**: Credit cards, lines of credit, loans
- **Investment accounts**: Brokerage, advisory, retirement accounts
- **Digital services**: Online banking, mobile banking, payment services

## Aggregates in This Context

### CustomerApplication Aggregate

- **Root**: CustomerApplication (ApplicationID)
- **Contains**: ApplicantProfile, DocumentRecord, VerificationResult
- **Value objects**: Address, TaxIdentifier, DateOfBirth
- **Key invariant**: Application cannot be approved without completing all required identity verification steps

### CustomerProfile Aggregate

- **Root**: CustomerProfile (CustomerID)
- **Contains**: KYCRecord, RiskClassification, ProductEnrollment
- **Value objects**: RiskRating, OnboardingStatus
- **Key invariant**: Customer cannot be activated without an approved KYC review; risk rating must be assigned before product enrollment

## Domain Events

- CustomerApplicationReceived -- new application submitted; triggers verification workflow
- IdentityVerified -- identity confirmed through document or electronic verification
- KYCCompleted -- due diligence process completed; risk rating assigned
- CustomerOnboarded -- customer fully activated; published to all downstream contexts
- ProductEnrolled -- customer enrolled in a specific product (triggers account creation in other contexts)
- CustomerRiskRatingChanged -- risk classification updated due to periodic review or new information
- CustomerRelationshipClosed -- customer off-boarded; accounts closed across all contexts

## Integration with Other Contexts

- **Risk & Compliance** (upstream): KYC screening, sanctions checks, and risk classification services
- **Accounts & Ledger** (downstream): CustomerOnboarded and ProductEnrolled events trigger account creation
- **Payments & Transfers** (downstream): Customer identity data used for payment validation and fraud screening
- **Lending & Credit** (downstream): Verified customer profiles used in loan applications
- **Trading & Investments** (downstream): Suitability profiles inform investment product eligibility

This context publishes the canonical customer identity through a **Published Language**, providing a standardized CustomerOnboarded event consumed by all downstream contexts. The event includes CustomerID, verified legal name, risk rating, and enrolled products.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- One of the six bounded contexts in the finance domain
- [Context Map](../context-map/) -- Published Language relationship to all downstream contexts
- [Domain Events](../domain-events/) -- CustomerOnboarded is a foundational event consumed across the institution
- [Risk & Compliance Context](../risk-compliance-context/) -- KYC and screening services provided by Risk & Compliance
- [Regulatory Compliance](../regulatory-compliance/) -- CIP, CDD, EDD requirements under BSA/AML; GDPR for data privacy
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between external identity verification services and internal models

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Financial Crimes Enforcement Network. "Customer Due Diligence Requirements for Financial Institutions." Federal Register, 2016.
- Wolfsberg Group. "Wolfsberg AML Guidance." https://www.wolfsberg-principles.com/
- FATF. "International Standards on Combating Money Laundering and the Financing of Terrorism & Proliferation." https://www.fatf-gafi.org/
