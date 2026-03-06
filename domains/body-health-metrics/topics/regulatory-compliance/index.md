# Regulatory Compliance for Body Health Metrics

## Overview

Regulatory compliance in the body health metrics domain addresses the legal, governance, and privacy requirements that shape how body health data is collected, stored, processed, shared, and retained. Health measurement data is among the most sensitive categories of personal information, subject to stringent regulations across jurisdictions. The domain model must incorporate compliance requirements as first-class domain concepts, not as afterthought infrastructure concerns.

## Applicable Regulations

### HIPAA (Health Insurance Portability and Accountability Act)

HIPAA governs the use and disclosure of Protected Health Information (PHI) in the United States. Body health measurements -- weight, blood pressure, body composition, fitness assessments -- constitute PHI when associated with an identifiable individual.

**Domain model implications**:

- **Minimum necessary standard**: The domain model exposes only the minimum data needed for each bounded context's function. The Health Scoring Context receives measurement values but does not need to know the measurement device serial number or administrator identity.
- **Access controls**: Each bounded context enforces role-based access constraints. Clinical Integration Context data access requires clinical authorization credentials.
- **Audit trail**: All data access and modification events are recorded. The domain model supports audit event generation as a domain concern, with PersonId, AccessorId, DataCategory, AccessType, and Timestamp.
- **Breach notification**: The domain model includes data classification attributes that enable rapid identification of affected individuals in the event of a data breach.
- **De-identification**: The domain model supports statistical de-identification by separating measurement data from personal identifiers, enabling population-level analysis without exposing individual PHI.

### GDPR (General Data Protection Regulation)

GDPR governs the processing of personal data for individuals in the European Union. Body health metrics constitute "special category data" (health data) under GDPR, requiring explicit consent and heightened protection.

**Domain model implications**:

- **Lawful basis**: The domain model tracks the lawful basis for processing each category of health data (explicit consent, vital interests, healthcare provision).
- **Purpose limitation**: Each bounded context processes data only for its stated purpose. Measurement data collected for personal health tracking cannot be repurposed for research without separate consent.
- **Data minimization**: Value objects carry only the attributes necessary for their domain function. Measurement value objects do not include personally identifiable metadata.
- **Right to erasure**: The domain model supports data deletion workflows. When an individual exercises their right to erasure, the system can identify and remove all personal health data across bounded contexts.
- **Data portability**: The Clinical Integration Context supports data export in machine-readable formats (FHIR resources) to fulfill data portability requests.
- **Consent management**: Consent is modeled as a domain concept with scope, granularity (per measurement type, per context), and revocability.

### FDA Considerations (United States)

If the body health metrics system provides clinical decision support or is marketed as a medical device, FDA regulations may apply. The domain model must distinguish between wellness features (not regulated) and clinical features (potentially regulated).

**Domain model implications**:

- **Intended use classification**: The Health Scoring Context must clearly classify its outputs as wellness information or clinical guidance, as this classification determines regulatory obligations.
- **Software validation**: Clinical-grade features require documented validation evidence. The domain model supports traceability from requirements through implementation to validation results.

### State and Provincial Regulations

Various jurisdictions impose additional requirements on health data handling, including data residency requirements, enhanced breach notification timelines, and sector-specific privacy rules.

## Compliance as Domain Concepts

### DataClassification Value Object

Every measurement and derived metric carries a data classification indicating its sensitivity level and applicable regulatory frameworks.

- **Attributes**: sensitivity level (public, internal, confidential, restricted), applicable regulations (HIPAA, GDPR, FDA), data category (biometric, clinical, wellness)
- **Usage**: Drives access control decisions, audit requirements, and data handling procedures across all bounded contexts

### ConsentRecord Entity

Consent is modeled as a domain entity with its own lifecycle, tracking an individual's authorization for specific data processing activities.

- **Identity**: ConsentRecordId
- **Attributes**: PersonId, consent scope (measurement types, contexts, purposes), grant date, expiration date, revocation status
- **Invariants**: Processing cannot occur without a valid, unrevoked consent record for the relevant scope. Expired consent must be renewed before processing resumes.

### AuditEvent Domain Event

Every significant data access or modification generates an audit event, providing the compliance trail required by HIPAA and GDPR.

- **Payload**: event type (access, create, update, delete, export), PersonId, accessor identity, data categories accessed, timestamp, justification
- **Retention**: Audit events are retained according to regulatory minimums (six years for HIPAA, duration of processing plus statute of limitations for GDPR)

### DataRetentionPolicy Value Object

Specifies how long each category of health data is retained and what happens at the end of the retention period.

- **Attributes**: data category, retention duration, retention justification (regulatory requirement, clinical necessity, individual consent), disposal method (deletion, anonymization, archival)

## Cross-Cutting Compliance Patterns

**Privacy by design**: Compliance requirements are incorporated into the domain model from the outset, not bolted on after development. Value objects carry classification metadata. Aggregates enforce access constraints. Events carry audit information.

**Pseudonymization**: The domain model supports separating personal identifiers from measurement data, enabling data processing and analysis without exposing individual identity.

**Data residency**: The domain model supports data residency metadata indicating where specific data may be stored and processed, enabling multi-region deployment compliance.

**Incident response**: The domain model supports breach assessment by enabling rapid identification of which individuals' data was potentially affected, what categories of data were involved, and what regulatory notification obligations apply.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. HIPAA Privacy Rule. https://www.hhs.gov/hipaa/
- European Union. General Data Protection Regulation (GDPR). Regulation (EU) 2016/679.
- U.S. Food and Drug Administration. *Policy for Device Software Functions and Mobile Medical Applications*. 2019.
