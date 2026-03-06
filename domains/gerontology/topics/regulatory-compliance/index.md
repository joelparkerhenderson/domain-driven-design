# Regulatory Compliance in the Gerontology Domain

## Overview

Regulatory compliance shapes the design of the gerontology domain model in fundamental ways. Healthcare for older adults is among the most heavily regulated areas of any industry, with requirements imposed at federal, state, and organizational levels. Domain-Driven Design must account for these regulations not as afterthoughts but as first-class domain concepts that influence aggregate design, invariant rules, domain events, and repository requirements.

Evans (2003) recognizes that legal and governance requirements are part of the domain and should be modeled explicitly. In the gerontology domain, regulatory requirements determine what data must be collected, how long it must be retained, who can access it, and what clinical processes must be followed. These requirements differ across care settings, jurisdictions, and payer types, adding complexity that the domain model must accommodate.

## HIPAA (Health Insurance Portability and Accountability Act)

HIPAA governs the privacy and security of protected health information (PHI) in the United States. In the gerontology domain, HIPAA compliance affects every bounded context because all contexts handle PHI.

The Privacy Rule requires that PHI be disclosed only for treatment, payment, and healthcare operations or with patient authorization. This requirement influences repository design, mandating access control mechanisms that restrict data retrieval to authorized users and purposes. The domain model must support role-based access that distinguishes between a geriatrician viewing a full assessment, a pharmacist viewing medication-relevant data, and a caregiver viewing care plan information.

The Security Rule requires administrative, physical, and technical safeguards for electronic PHI. While infrastructure implementation is outside the domain model, the model must support audit trail requirements. Every repository must record who accessed or modified data, when, and for what purpose. This requirement is modeled as a cross-cutting concern affecting all aggregate persistence operations.

The Breach Notification Rule requires notification of unauthorized PHI disclosure. The domain model includes domain events for security-relevant activities that support breach detection and notification workflows.

## CMS (Centers for Medicare and Medicaid Services) Requirements

CMS regulations impose specific assessment, documentation, and reporting requirements that directly shape the domain model.

### Minimum Data Set (MDS)

The MDS is a standardized assessment instrument required by CMS for all residents of Medicare and Medicaid-certified nursing facilities. The MDS covers cognitive patterns, communication, mood, behavior, functional status, continence, disease diagnoses, health conditions, nutritional status, oral/dental status, skin conditions, activity participation, medications, and special treatments. The GeriatricAssessment aggregate in the Geriatric Assessment Context must capture data elements that map to MDS sections, even though the domain model uses its own ubiquitous language rather than MDS terminology. The anti-corruption layer translates between the domain model and MDS reporting format.

### OASIS (Outcome and Assessment Information Set)

OASIS is required for patients receiving Medicare home health services. It covers demographics, patient history, living arrangements, sensory status, integumentary status, respiratory status, cardiac status, elimination status, neuro/emotional/behavioral status, ADL/IADL, medications, and equipment management. The Functional Independence Context and Geriatric Assessment Context must support OASIS data collection while maintaining their own domain models.

### PEPPER (Program for Evaluating Payment Patterns Electronic Report)

PEPPER reports identify potential payment errors in Medicare claims. The domain model must support accurate documentation that substantiates clinical decisions, reducing the risk of audit findings. This requirement influences the level of detail captured in assessment aggregates and care plan entities.

### Hospice Benefit Requirements

The Medicare Hospice Benefit requires certification that a patient has a terminal illness with a prognosis of six months or less. It mandates an interdisciplinary team, a plan of care, and specific documentation for each certification period. These requirements directly shape the HospiceEnrollment aggregate and PalliativeCarePlan aggregate in the End of Life Planning Context.

## State Regulations

### Advance Directive Laws

Advance directive requirements vary by state. Some states require two witnesses, others require notarization, and some accept either. The AdvanceDirective aggregate must accommodate jurisdictional variations in legal validity requirements. The invariant that enforces legal compliance must be configurable by jurisdiction.

### POLST Program Regulations

POLST programs are implemented at the state level with varying requirements for form content, authorized signers, and portability. The POLSTOrder entity must model the requirements of the applicable state POLST program.

### Elder Abuse Reporting

Most states mandate that healthcare professionals report suspected elder abuse, neglect, or exploitation. The domain model should support flagging and documenting concerns that trigger mandatory reporting obligations, including domain events that initiate reporting workflows.

### Scope of Practice Regulations

State scope-of-practice laws determine which clinicians can perform specific assessments and prescribe specific treatments. The domain model must validate that clinical actions are performed by appropriately credentialed providers, enforcing scope-of-practice constraints through invariant rules on assessment and prescribing aggregates.

## Quality Reporting Requirements

### MIPS (Merit-based Incentive Payment System)

MIPS requires reporting on quality measures, promoting interoperability, improvement activities, and cost. Several MIPS quality measures are directly relevant to geriatric care, including fall screening, medication reconciliation, and advance care planning. The domain model must capture data elements necessary for MIPS reporting.

### Nursing Home Quality Reporting

CMS publishes nursing home quality ratings based on health inspection results, staffing, and quality measures derived from MDS data. The domain model supports the capture of data elements that feed into these quality measures.

## Compliance as a Cross-Cutting Concern

Regulatory compliance is not isolated in a single bounded context but influences all contexts. The domain model addresses compliance through several mechanisms: invariant rules that enforce required data collection, repository audit trails that satisfy HIPAA documentation requirements, domain events that trigger compliance-related workflows (such as mandatory reporting), value objects that capture regulatory classifications (such as MDS assessment categories), and configurable rules that accommodate jurisdictional variations.

Khononov (2021) emphasizes that compliance requirements should be modeled explicitly in the domain rather than treated as external constraints applied after the fact. By making regulatory requirements visible in the domain model, the system ensures that compliance is built into clinical workflows rather than enforced as an afterthought.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- U.S. Department of Health and Human Services. (1996). Health Insurance Portability and Accountability Act (HIPAA).
- Centers for Medicare and Medicaid Services. (2023). Minimum Data Set (MDS) 3.0.
- Centers for Medicare and Medicaid Services. (2023). OASIS-E Guidance Manual.
