# Regulatory Compliance - Dental Domain

## Overview

Regulatory compliance requirements shape the domain model design in the dental domain by imposing mandatory rules for patient data protection, clinical documentation, informed consent, and insurance billing practices. These regulations come from federal laws (HIPAA), state dental practice acts, professional standards (ADA principles), and insurance industry requirements. The domain model must encode compliance rules as business logic within the appropriate bounded contexts rather than treating compliance as an afterthought.

## HIPAA Compliance

The Health Insurance Portability and Accountability Act (HIPAA) establishes national standards for protecting patient health information. HIPAA compliance affects every bounded context in the dental domain.

### Privacy Rule (45 CFR Part 164, Subparts A and E)

The Privacy Rule governs how protected health information (PHI) is used and disclosed. Domain model implications include:

- Minimum necessary standard: Each bounded context should access only the PHI required for its specific function. The Practice Management Context needs demographic and insurance data but does not need detailed clinical charting data. This aligns with bounded context separation.
- Patient access rights: Patients have the right to access their dental records. The domain model must support complete record compilation across bounded contexts when a patient requests their records.
- Accounting of disclosures: The system must track when PHI is shared with external parties. Domain events that cross system boundaries to insurance clearinghouses or referral providers must be logged for disclosure accounting.
- Authorization requirements: Certain uses of PHI require explicit patient authorization beyond the treatment, payment, and operations exceptions.

### Security Rule (45 CFR Part 164, Subparts A and C)

The Security Rule requires administrative, physical, and technical safeguards for electronic PHI. While primarily an infrastructure concern, the domain model supports security through:

- Access control modeling: Each bounded context defines role-based access requirements. Dental hygienists may need read-write access to periodontal probing data but read-only access to treatment plans.
- Audit trail requirements: Domain events provide a natural audit trail of all data access and modifications within the system.
- Data integrity: Aggregate invariants and value object validation ensure that clinical data maintains integrity within each bounded context.

### Breach Notification Rule

The domain model supports breach detection by maintaining comprehensive event logs that can identify the scope of affected records if a breach occurs.

## State Dental Practice Acts

Each state maintains a dental practice act that regulates the practice of dentistry within its jurisdiction. These acts affect the domain model in several ways:

### Scope of Practice

State laws define which procedures different dental professionals may perform. The domain model must enforce that:

- Dental hygienists can perform prophylaxis, scaling, and radiographic imaging but cannot diagnose or prescribe.
- Dental assistants have varying expanded function permissions by state (some states allow assistants to place restorations under direct supervision).
- Dentists are the only professionals authorized to diagnose dental conditions and prescribe treatment.

The provider entity in each bounded context carries licensure type and state credentials that determine permissible actions within the system.

### Supervision Requirements

State laws specify supervision levels (direct, indirect, general) required for procedures performed by allied dental professionals. The domain model tracks supervision relationships to ensure compliance.

### Record Retention

State dental practice acts specify minimum retention periods for dental records (commonly 7-10 years from the last treatment date, longer for minors). The domain model must support record retention policies and prevent premature record destruction.

## Informed Consent Requirements

Informed consent is both an ethical obligation and a legal requirement. The domain model in the Treatment Planning Context enforces:

- Documentation that the patient was informed of the proposed procedure, material risks, alternatives including no treatment, and expected outcomes.
- Provider attribution identifying who obtained consent.
- Temporal validation ensuring consent was obtained before procedure initiation.
- Re-consent requirements when treatment plans are significantly modified.
- Special consent requirements for specific procedures (surgical extractions, implant placement, sedation).

State laws vary in their specific informed consent requirements, and the domain model must accommodate state-specific consent documentation rules.

## Insurance Billing Compliance

Dental billing compliance requirements come from federal and state anti-fraud statutes and insurance contract terms:

### Accurate Coding

The domain model enforces that CDT codes submitted on claims accurately represent the procedures performed. Upcoding (submitting a higher-reimbursement code than the procedure performed) is a compliance violation. The Restorative Services Context records the actual procedure performed, and the Practice Management Context must use the corresponding code.

### Bundling and Unbundling

Certain procedures are considered inclusive of other procedures and should not be billed separately. For example, a post-operative visit within the global period of a surgical extraction should not generate a separate claim. The domain model maintains bundling rules that prevent prohibited unbundling of procedures.

### Pre-Authorization

Some insurance plans require pre-authorization for major procedures. The domain model tracks pre-authorization status for applicable treatment items and prevents claim submission without required authorizations.

### Timely Filing

Insurance contracts specify deadlines for claim submission (typically 90-365 days from service date). The Insurance Claim aggregate tracks filing deadlines and raises events when claims approach their filing limits.

## Dental-Specific Regulatory Standards

### Infection Control (OSHA and CDC Guidelines)

While primarily operational, infection control standards affect the domain model by requiring documentation of sterilization cycles, instrument tracking, and exposure incident records. The Practice Management Context may track sterilization logs and instrument cassette assignments.

### Radiation Safety

State radiation safety regulations require documentation of radiographic equipment inspection, operator certification, and patient radiation exposure logs. The Clinical Examination Context's radiographic management must support these documentation requirements.

## References

- U.S. Department of Health and Human Services. HIPAA Privacy Rule. 45 CFR Part 164.
- U.S. Department of Health and Human Services. HIPAA Security Rule. 45 CFR Part 164.
- American Dental Association. Principles of Ethics and Code of Professional Conduct. ADA, updated periodically.
- Centers for Disease Control and Prevention. Guidelines for Infection Control in Dental Health-Care Settings. MMWR, 2003.
- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
