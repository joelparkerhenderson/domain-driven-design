# Regulatory Compliance in Oncology

## Overview

Regulatory compliance in oncology encompasses the legal, governance, and accreditation requirements that shape domain model design and constrain system behavior. Cancer care operates under a dense regulatory framework that includes federal privacy laws, FDA drug safety regulations, NCI reporting requirements, clinical trial protocols, accreditation standards, and state-level cancer reporting mandates. The oncology domain model must incorporate these regulatory requirements as first-class domain concepts, not as afterthoughts bolted onto the business logic.

## HIPAA (Health Insurance Portability and Accountability Act)

HIPAA establishes the baseline for protecting patient health information in the United States. In the oncology domain, HIPAA compliance affects how patient data is shared between bounded contexts, how domain events carrying patient information are transmitted and stored, and how anti-corruption layers handle data exchange with external systems.

The domain model reflects HIPAA requirements in several ways. Patient identity information is treated as a sensitive value object with access controls defined at the domain level. Domain events carrying protected health information (PHI) are designed with minimum necessary data principles: events include only the PHI required for the consuming context to perform its function. The shared kernel, which includes patient identity, defines the scope of PHI that is available across all contexts.

HIPAA's audit trail requirements align naturally with event-driven architecture. Domain events provide an immutable record of all data access and modification, supporting the audit requirements. The event store serves double duty as both the domain's state management mechanism and the compliance audit trail.

## FDA Regulations

The FDA regulates drugs, biological products, and medical devices used in oncology treatment. FDA regulations affect the oncology domain model in several areas.

Drug safety reporting requirements influence the Chemotherapy Management Context. When serious adverse events occur during treatment with FDA-regulated therapies, the domain model must support the generation of adverse event reports (MedWatch/FDA Form 3500A). The Toxicity Assessment aggregate captures the data elements required for FDA safety reporting, including the suspect drug, the adverse event description and outcome, and the causality assessment.

FDA regulations governing clinical trials (21 CFR Part 11) affect the Treatment Planning Context's clinical trial enrollment aggregate. Electronic records related to clinical trial data must meet specific requirements for data integrity, including electronic signatures, audit trails, and data validation. The domain model's event-sourced design supports these requirements by maintaining a complete, immutable history of all clinical trial-related data changes.

Radiation-emitting device regulations affect the Radiation Therapy Context. Treatment delivery systems are FDA-regulated medical devices, and the domain model must capture the device identification and calibration status as part of treatment fraction records.

## NCI Reporting Requirements

The National Cancer Institute (NCI) requires cancer incidence reporting through the Surveillance, Epidemiology, and End Results (SEER) program and the state-based cancer registry system. These reporting requirements affect the Diagnosis and Staging Context, which must capture and report standardized data elements including demographic information, tumor site and histology (ICD-O coding), staging at diagnosis, first course of treatment, and vital status follow-up.

The domain model supports NCI reporting through the anti-corruption layer between the Diagnosis and Staging Context and tumor registry systems. The ACL translates the domain model's rich diagnostic representation into the standardized coding formats required by NAACCR (North American Association of Central Cancer Registries) data standards.

## Clinical Trial Regulations

Clinical trials in oncology are governed by FDA regulations (21 CFR Parts 50, 56, 312), Good Clinical Practice (GCP) guidelines (ICH E6), and institutional review board (IRB) oversight. These regulations impose specific requirements on the domain model.

Informed consent tracking is a domain requirement. The Clinical Trial Enrollment aggregate must verify that informed consent is documented before enrollment proceeds. The consent status is a domain invariant, not merely a process step.

Protocol compliance tracking requires the domain model to capture whether treatment was administered according to the trial protocol, including any protocol deviations and their justification. The Chemotherapy Management Context must distinguish between protocol-mandated dose modifications and investigator-initiated modifications.

Adverse event reporting in clinical trials follows specific timelines and formats defined by the trial sponsor and the FDA. Serious adverse events must be reported within defined timeframes. The domain model must support these reporting timelines through domain events that trigger reporting workflows.

## Accreditation Standards

Cancer center accreditation by the Commission on Cancer (CoC) of the American College of Surgeons imposes standards that influence the domain model. Key CoC standards affecting the domain include the requirement for multidisciplinary tumor board review (reflected in the Treatment Planning Context's tumor board case presentation workflow), survivorship care plan delivery (reflected in the Survivorship Care Context's care plan generation), and quality measure reporting.

The National Accreditation Program for Breast Centers (NAPBC), the National Accreditation Program for Rectal Cancer (NAPRC), and similar program-specific accreditations impose additional standards that may require domain model extensions for specific cancer types.

## State Cancer Reporting Laws

Most states mandate cancer case reporting to state cancer registries. Reporting requirements vary by state but generally require notification within a specified timeframe after diagnosis. The domain model supports this through the DiagnosisConfirmed domain event, which can trigger state-specific reporting workflows through the tumor registry anti-corruption layer.

## Quality Measure Reporting

Oncology quality measures from organizations including ASCO's Quality Oncology Practice Initiative (QOPI), CMS merit-based incentive payment system (MIPS), and the National Quality Forum (NQF) require tracking specific clinical process and outcome measures. These quality measures are best served by read models built from domain events, following the CQRS pattern. The write-side domain model captures the clinical data; the read-side projections calculate and report quality measures.

## Compliance as Domain Logic

Regulatory compliance in oncology should be treated as domain logic, not as a cross-cutting infrastructure concern. Compliance rules are part of the business domain: they constrain what clinical actions are permissible, define what data must be captured, and specify how information must be reported. The domain model should enforce compliance invariants within aggregates, not rely on external compliance checking systems.

For example, the requirement that informed consent must precede clinical trial enrollment is an aggregate invariant in the Clinical Trial Enrollment aggregate. The requirement that adverse events must be graded using CTCAE terminology is a value object validation rule. The requirement for tumor board review before treatment plan approval is a state transition constraint in the Treatment Plan aggregate.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. HIPAA Privacy Rule. 45 CFR Parts 160, 164.
- U.S. Food and Drug Administration. 21 CFR Part 11: Electronic Records; Electronic Signatures.
- National Cancer Institute. SEER Program. https://seer.cancer.gov
- American College of Surgeons. Commission on Cancer Standards. https://www.facs.org/quality-programs/cancer-programs
