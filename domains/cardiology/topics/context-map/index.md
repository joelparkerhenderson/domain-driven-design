# Context Map in Cardiology

## Overview

A context map provides a visual and conceptual representation of the relationships and interactions between bounded contexts in the cardiology domain. It documents how each context communicates with others, which context is upstream or downstream, and what integration patterns govern their interactions. The context map is a strategic design artifact that helps teams understand dependencies and coordination requirements across the cardiology system.

## Context Relationships

### Clinical Assessment Context (Upstream)

The Clinical Assessment Context serves as the primary upstream context in the cardiology domain. It produces patient assessment data, risk stratification results, and clinical decision outputs that flow downstream to other contexts.

- **To Diagnostic Imaging Context**: Customer-Supplier relationship. Clinical Assessment publishes imaging referral events containing clinical indications and relevant history. Diagnostic Imaging consumes these to create study orders. The imaging context conforms to the clinical assessment's referral model.

- **To Interventional Procedures Context**: Customer-Supplier relationship. Clinical Assessment publishes catheterization referrals with risk scores and clinical findings. The interventional context uses these to initiate procedural planning.

- **To Heart Failure Management Context**: Shared Kernel relationship. Both contexts share a common model for patient vital signs, biomarker values, and functional classification. Changes to the shared kernel require agreement from both teams.

- **To Cardiac Rehabilitation Context**: Customer-Supplier relationship. Clinical Assessment produces risk profiles and exercise capacity data that the rehabilitation context consumes for program design.

### Diagnostic Imaging Context

The Diagnostic Imaging Context operates as both a consumer of clinical referrals and a supplier of structured imaging results.

- **To Clinical Assessment Context**: Published Language relationship. Imaging results are published using standardized reporting structures (e.g., ASE echocardiography measurement guidelines) that the Clinical Assessment Context consumes.

- **To Interventional Procedures Context**: Customer-Supplier relationship. Imaging findings such as valve gradients, coronary anatomy, and ventricular function measurements directly inform procedural planning.

- **To Heart Failure Management Context**: Published Language relationship. Serial echocardiographic measurements of ejection fraction and chamber dimensions are published in a standardized format consumed by the heart failure context for longitudinal tracking.

### Interventional Procedures Context

- **To Clinical Assessment Context**: Conformist relationship for post-procedural follow-up data. The interventional context publishes procedural outcomes that the clinical assessment context receives in its own format.

- **To Electrophysiology Context**: Partnership relationship for hybrid procedures requiring both interventional and electrophysiology expertise (e.g., left atrial appendage closure with concurrent ablation).

### Electrophysiology Context

- **To Heart Failure Management Context**: Customer-Supplier relationship. Electrophysiology provides CRT implantation and optimization data that the heart failure context consumes for device-based therapy management.

- **To Clinical Assessment Context**: Conformist relationship. EP study results and device interrogation summaries are published upstream.

### Heart Failure Management Context

- **To Cardiac Rehabilitation Context**: Customer-Supplier relationship. Heart failure management produces GDMT optimization status, functional class assessments, and exercise clearance decisions that cardiac rehabilitation consumes.

- **To Interventional Procedures Context**: Customer-Supplier relationship for MCS device referrals.

### Cardiac Rehabilitation Context (Downstream)

The Cardiac Rehabilitation Context is the most downstream context, consuming data from Clinical Assessment, Diagnostic Imaging, Heart Failure Management, and Interventional Procedures to design and monitor rehabilitation programs.

## External System Integration

- **EHR Systems**: Anti-Corruption Layer protects all cardiology contexts from the EHR's generic clinical data model.
- **Laboratory Information Systems**: Anti-Corruption Layer translates lab results into cardiac biomarker value objects.
- **Pharmacy Systems**: The Heart Failure Management Context uses an Anti-Corruption Layer to integrate medication data.
- **Billing/Coding Systems**: Generic subdomain with Open Host Service providing CPT and ICD-10 code lookup.

## Integration Patterns Summary

| Upstream Context | Downstream Context | Pattern |
|---|---|---|
| Clinical Assessment | Diagnostic Imaging | Customer-Supplier |
| Clinical Assessment | Interventional Procedures | Customer-Supplier |
| Clinical Assessment | Heart Failure Management | Shared Kernel |
| Diagnostic Imaging | All consuming contexts | Published Language |
| Interventional Procedures | Electrophysiology | Partnership |
| Heart Failure Management | Cardiac Rehabilitation | Customer-Supplier |
| Electrophysiology | Heart Failure Management | Customer-Supplier |

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
- Brandolini, Alberto. *Introducing EventStorming*. Leanpub, 2021.
