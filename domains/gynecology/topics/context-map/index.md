# Context Map for the Gynecology Domain

## Overview

A context map is a visual and textual representation of the relationships and interactions between bounded contexts. It documents how each context integrates with others, what data flows between them, and what organizational or technical patterns govern those integrations. In the gynecology domain, the context map captures the clinical workflows that cross context boundaries.

## Purpose

Clinical gynecology workflows rarely stay within a single bounded context. A patient may present for a routine screening, receive an abnormal result, undergo clinical assessment, be referred for surgery, and receive patient education at every step. The context map makes these cross-context flows explicit, preventing hidden dependencies and ensuring that integration points are deliberately designed.

## Context Relationships

### Clinical Assessment Context -- Reproductive Health Context

- Relationship type: Customer-Supplier
- The Clinical Assessment Context is upstream. When a clinical encounter produces findings relevant to reproductive health (such as a menstrual irregularity or fertility concern), it publishes a DiagnosticImpressionFormed event.
- The Reproductive Health Context is downstream. It consumes diagnostic impressions and uses them to initiate or update reproductive health plans.

### Clinical Assessment Context -- Surgical Services Context

- Relationship type: Customer-Supplier
- The Clinical Assessment Context is upstream. When a diagnostic workup concludes that surgical intervention is indicated, it publishes a SurgicalReferralCreated event.
- The Surgical Services Context is downstream. It consumes referrals and creates surgical cases.

### Clinical Assessment Context -- Prenatal Care Context

- Relationship type: Partnership
- These contexts collaborate as equals. The Clinical Assessment Context performs initial pregnancy confirmation and early assessments. The Prenatal Care Context takes over ongoing pregnancy management. Both contexts share a published language for pregnancy-related clinical findings.

### Screening Programs Context -- Clinical Assessment Context

- Relationship type: Customer-Supplier
- The Screening Programs Context is upstream for abnormal results. When a screening test returns an abnormal result, it publishes an AbnormalResultDetected event.
- The Clinical Assessment Context is downstream. It consumes abnormal screening results and initiates follow-up diagnostic workups.

### Surgical Services Context -- Clinical Assessment Context

- Relationship type: Customer-Supplier (reverse flow)
- The Surgical Services Context is upstream for postoperative data. When surgery is completed, it publishes a SurgeryCompleted event.
- The Clinical Assessment Context is downstream. It consumes surgical outcomes and schedules postoperative follow-up assessments.

### Prenatal Care Context -- Screening Programs Context

- Relationship type: Customer-Supplier
- The Prenatal Care Context is upstream. It requests prenatal screening tests (such as glucose tolerance, group B streptococcus) through the Screening Programs Context.
- The Screening Programs Context is downstream for test execution but upstream for result delivery.

### Patient Education Context -- All Clinical Contexts

- Relationship type: Conformist
- The Patient Education Context conforms to the models of the clinical contexts. It consumes events from Clinical Assessment, Reproductive Health, Surgical Services, Prenatal Care, and Screening Programs to determine what educational content to deliver.
- It does not impose its own model on upstream contexts.

## Integration Patterns Used

- Published Language: Clinical Assessment and Prenatal Care share a published language for pregnancy findings.
- Open Host Service: The Screening Programs Context exposes a standardized interface for test ordering and result retrieval.
- Anti-Corruption Layer: The Surgical Services Context uses an ACL when integrating with external operating room scheduling systems.
- Shared Kernel: Clinical Assessment and Prenatal Care share a small kernel of patient demographic and clinical history value objects.

## External System Integration

- Laboratory Information Systems: The Screening Programs Context integrates with external lab systems through an anti-corruption layer that translates lab-specific codes into domain screening result value objects.
- Radiology Systems: The Prenatal Care Context integrates with ultrasound and imaging systems, translating DICOM-based data into domain ultrasound assessment value objects.
- Electronic Health Records: All contexts integrate with the EHR through anti-corruption layers that prevent EHR data structures from leaking into domain models.

## Data Flow Summary

1. Patient presents for screening. Screening Programs Context manages the episode.
2. Abnormal result detected. AbnormalResultDetected event flows to Clinical Assessment Context.
3. Diagnostic workup performed. DiagnosticImpressionFormed event flows to relevant downstream context.
4. If surgery indicated, SurgicalReferralCreated event flows to Surgical Services Context.
5. Post-surgery, SurgeryCompleted event flows back to Clinical Assessment Context.
6. At each step, relevant events flow to Patient Education Context to trigger appropriate content.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context map patterns.
