# Treatment Planning Context - Dental Domain

## Overview

The Treatment Planning Context translates clinical examination findings into actionable, sequenced treatment plans that patients can understand, evaluate, and approve. This context bridges the gap between clinical diagnosis and procedure execution, managing the critical communication and decision-making process between dentist and patient. It handles treatment sequencing based on clinical priorities and dependencies, case presentation for patient communication, informed consent documentation, and cost estimation incorporating insurance benefits.

## Core Responsibilities

### Treatment Sequencing

Treatment sequencing organizes proposed procedures into a clinically appropriate order based on urgency, prerequisite dependencies, and healing requirements. The sequencing logic enforces rules such as:

- Emergency and pain-relief procedures are scheduled first.
- Extractions of non-restorable teeth precede prosthetic replacement planning.
- Periodontal disease must be stabilized before definitive restorative work.
- Endodontic treatment on a tooth precedes crown placement on that tooth.
- Foundation or core buildup precedes crown preparation.
- Implant placement requires adequate healing time (typically 3-6 months) before prosthetic loading.

Treatment plans are typically organized into phases: urgent care, disease control, restorative, and maintenance. Each phase must be substantially completed before the next phase begins, though some parallel scheduling is clinically appropriate.

### Case Presentation

Case presentation packages clinical findings and treatment recommendations into a format that enables informed patient decision-making. A case presentation includes a summary of current conditions found during examination, the consequences of non-treatment for each condition, treatment options with their relative advantages and disadvantages, the recommended treatment sequence with timeline, and the estimated financial breakdown.

The case presentation is prepared by the clinical team and delivered during a dedicated appointment or as part of an examination visit. It serves as the foundation for the informed consent process.

### Informed Consent

Informed consent management ensures that patients provide documented agreement before treatment proceeds. The consent process requires disclosure of the proposed procedure, expected benefits, material risks, alternatives including no treatment, and the opportunity for the patient to ask questions. Each treatment item in the plan requires associated consent documentation before the downstream clinical context can initiate the procedure.

Consent records are linked to specific treatment items and capture the date, the provider who obtained consent, the specific risks and alternatives discussed, and confirmation of patient understanding. Consent may need to be re-obtained if treatment plans are significantly modified.

### Cost Estimation

Cost estimation calculates the expected financial impact of a treatment plan by applying the practice fee schedule, estimating insurance coverage based on verified benefits, and determining the patient's out-of-pocket responsibility. The estimation accounts for:

- Practice fees for each CDT procedure code.
- Insurance coverage percentages by benefit category (preventive, basic, major, orthodontic).
- Annual maximum benefit remaining.
- Deductible amounts applied and remaining.
- Frequency limitations (some procedures are covered only at specific intervals).
- Pre-authorization requirements for major procedures.

Cost estimates are presented as part of the case presentation and updated when insurance verification provides new benefit information.

## Aggregate Roots

**Treatment Plan**: Contains sequenced treatment items organized into phases. Enforces sequencing invariants and consent requirements. Tracks plan status through draft, presented, approved, in-progress, and completed states.

**Case Presentation**: Groups the clinical findings, treatment recommendations, and financial estimates prepared for patient communication. Linked to a specific treatment plan.

## Key Entities

- Treatment Item: A single proposed procedure with CDT code, tooth designation, priority, dependencies, and cost estimate.
- Consent Record: Documentation of patient agreement to specific treatment items.

## Key Value Objects

- CDT Procedure Code: Standardized procedure identifier with category and nomenclature.
- Cost Estimate: Composite value with gross fee, insurance estimate, and patient responsibility.
- Treatment Phase: Phase classification (urgent, disease control, restorative, maintenance).
- Sequencing Dependency: Link between treatment items indicating prerequisite relationship.

## Domain Events Produced

- TreatmentPlanApproved: Initiates procedure scheduling in downstream clinical contexts.
- InformedConsentObtained: Authorizes specific procedures to proceed.
- TreatmentPlanModified: Notifies downstream contexts of plan changes.
- CostEstimateUpdated: Reflects new financial calculations after insurance verification.

## Domain Events Consumed

- ExaminationCompleted: Provides diagnostic findings for treatment plan development.
- CariesDetected: Identifies specific restorative treatment needs.
- RadiographInterpreted: Provides imaging findings for planning.
- InsuranceVerified: Enables accurate cost estimation with verified benefits.
- RestorationCompleted: Marks treatment items as completed.
- EndodonticTreatmentCompleted: Enables sequencing of follow-up restorative items.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Dental Association. Principles of Ethics and Code of Professional Conduct. ADA, updated periodically.
