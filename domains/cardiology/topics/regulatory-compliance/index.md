# Regulatory Compliance in Cardiology

## Overview

Regulatory compliance is a cross-cutting concern that shapes domain model design across all six cardiology bounded contexts. The cardiology domain operates under federal healthcare regulations, professional licensing requirements, device safety mandates, clinical trial oversight, and quality reporting obligations. In Domain-Driven Design, regulatory requirements influence aggregate invariants, domain event payloads, repository query capabilities, and anti-corruption layer designs.

## HIPAA Compliance

The Health Insurance Portability and Accountability Act establishes requirements for protected health information (PHI) that affect every bounded context:

### Privacy Rule

- **Minimum Necessary Standard**: Domain models must support data access controls that limit PHI exposure to the minimum necessary for each role. A cardiac rehabilitation exercise specialist needs exercise capacity and vital sign data but not detailed catheterization angiographic findings.
- **Patient Authorization**: The domain must track patient authorization for uses and disclosures of PHI beyond treatment, payment, and healthcare operations. Research use of cardiology data requires explicit authorization modeling.
- **Accounting of Disclosures**: Domain events that transmit PHI across context boundaries must be logged for disclosure accounting. When a CriticalBiomarkerDetected event is published, the disclosure must be auditable.

### Security Rule

- **Access Controls**: Repository implementations must enforce role-based access controls. The domain model defines the access policy; the infrastructure layer implements it.
- **Audit Logging**: All access to aggregate roots containing PHI must generate audit log entries. The domain model includes audit metadata (who accessed what, when, for what purpose).
- **Data Integrity**: Aggregate invariants that protect clinical data integrity also serve HIPAA's data integrity requirements. An EjectionFraction value object's immutability ensures the measurement cannot be silently altered.

### Breach Notification

- **Breach Detection**: Domain events can serve as breach detection signals. Unusual patterns of data access (e.g., a user accessing patient records outside their assigned patient panel) can trigger security events.
- **Breach Documentation**: When a breach is identified, the domain must support documentation of affected records, individuals, and scope of exposure.

## CMS Regulations

The Centers for Medicare and Medicaid Services imposes requirements that directly affect cardiology domain models:

### Quality Reporting

- **MIPS (Merit-based Incentive Payment System)**: Cardiology practices must report quality measures that draw data from multiple bounded contexts. The domain model must support extraction of quality measure numerator and denominator data. Examples include: door-to-balloon time for STEMI patients, beta-blocker prescribing for post-MI patients, and cardiac rehabilitation referral rates.
- **APMs (Alternative Payment Models)**: Bundled payment models for cardiac procedures require the domain to track episode costs and outcomes across contexts. A PCI episode may span Clinical Assessment, Diagnostic Imaging, Interventional Procedures, and Cardiac Rehabilitation.

### Conditions of Participation

- **Medical Record Documentation**: Clinical assessments, imaging reports, procedural notes, and rehabilitation session records must meet CMS documentation standards. Domain aggregates include documentation completeness checks as invariants.
- **Informed Consent**: The Interventional Procedures and Electrophysiology contexts must model informed consent as a prerequisite for procedural aggregates. A CatheterizationProcedure aggregate cannot transition to "in-progress" without documented informed consent.
- **Discharge Planning**: Heart failure patients require documented discharge plans with follow-up appointments, medication reconciliation, and patient education. The Heart Failure Management Context models discharge planning as part of the care episode aggregate.

### Coverage Determinations

- **National Coverage Decisions (NCDs)**: CMS defines coverage criteria for specific cardiology procedures and devices. The domain model must validate that clinical indications meet NCD criteria. For example, ICD implantation requires documented LVEF <=35%, NYHA class II-III, and at least 40 days post-MI or 90 days post-revascularization.
- **Local Coverage Determinations (LCDs)**: Medicare Administrative Contractors may impose additional coverage criteria that vary by region. The domain must accommodate configurable coverage rules.

## FDA Device Regulations

Cardiac devices are regulated by the Food and Drug Administration:

### Unique Device Identification (UDI)

All implanted cardiac devices (stents, valves, pacemakers, ICDs, leads) must be tracked by their UDI. The DeviceImplantation and CIEDImplantation aggregates must include UDI as a mandatory attribute. This supports device recall management, post-market surveillance, and adverse event reporting.

### Post-Market Surveillance

- **MDR (Medical Device Reporting)**: Adverse events involving cardiac devices must be reported to the FDA. Domain events like ProceduralComplicationOccurred and LeadIntegrityCompromised may trigger MDR obligations.
- **Device Recalls and Advisories**: The CIEDImplantation repository must support queries by device manufacturer and model to identify patients affected by recalls or safety advisories.

### Premarket Requirements

Clinical trial data supporting new devices or indications must be managed with regulatory rigor. The domain must distinguish between commercially approved devices and investigational devices used under IDE (Investigational Device Exemption) protocols.

## Clinical Trial Regulations

Cardiology is a major clinical trial specialty. When the domain model supports clinical trial activities:

- **21 CFR Part 11**: Electronic records and signatures used in clinical trial data must meet FDA requirements for authentication, audit trails, and data integrity.
- **Good Clinical Practice (GCP)**: Source data verification, adverse event reporting, and protocol adherence tracking must be supported by domain models.
- **IRB Oversight**: Institutional Review Board approval status must be tracked for research-related data access and usage.

## State Regulations

- **Practice Acts**: State medical practice acts define scope of practice for cardiologists, nurse practitioners, physician assistants, and cardiac sonographers. The domain's access control model must reflect these scope boundaries.
- **Prescription Drug Monitoring**: State PDMP requirements may affect GDMT prescribing workflows in the Heart Failure Management Context.
- **Mandatory Reporting**: Some states require mandatory reporting of specific cardiac conditions (e.g., certain arrhythmias affecting driving safety).

## Regulatory Compliance in Domain Design

Regulatory requirements are encoded in the domain model through:

1. **Aggregate invariants**: Consent must precede procedures; UDI must be recorded for implants.
2. **Domain events**: Audit-relevant events carry compliance metadata.
3. **Value objects**: PHI classification tags, consent status, and coverage determination results.
4. **Domain services**: Coverage criteria validation, quality measure calculation, and adverse event classification.
5. **Repository queries**: Device recall identification, quality measure population extraction, and audit trail retrieval.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. HIPAA Privacy, Security, and Breach Notification Rules.
- CMS Quality Payment Program. MIPS Measures for Cardiology, 2024.
- FDA. Unique Device Identification System (UDI System). 21 CFR Part 830.
- FDA. 21 CFR Part 11: Electronic Records; Electronic Signatures.
