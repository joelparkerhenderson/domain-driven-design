# Context Map in Oncology

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a system. In the oncology domain, the context map documents how the six bounded contexts communicate, which contexts are upstream or downstream, and what integration patterns govern their interactions. The context map is essential for understanding the overall architecture of the oncology system and for identifying potential friction points where misaligned models could cause clinical errors.

## Context Relationships

### Diagnosis and Staging to Treatment Planning

Relationship type: Customer-Supplier. The Diagnosis and Staging Context is the upstream supplier, providing confirmed diagnoses, staging results, and molecular profiles. The Treatment Planning Context is the downstream customer, consuming this information to formulate treatment strategies.

The Treatment Planning Context depends on the accuracy and completeness of staging data. When a diagnosis is confirmed or a stage is revised, the Diagnosis and Staging Context publishes a DiagnosisConfirmed or StageRevised domain event. The Treatment Planning Context subscribes to these events and translates the staging data into its own model, which includes treatment-relevant risk stratification.

### Treatment Planning to Chemotherapy Management

Relationship type: Customer-Supplier. The Treatment Planning Context is upstream, providing approved treatment plans that specify systemic therapy modalities. The Chemotherapy Management Context is downstream, translating approved plans into specific chemotherapy orders with calculated doses and schedules.

The chemotherapy context does not blindly accept treatment plan directives. It applies its own domain rules for dose calculation, drug interaction checking, and schedule feasibility. If a planned regimen cannot be safely administered, the chemotherapy context raises a domain event that flows back to the treatment planning context for review.

### Treatment Planning to Radiation Therapy

Relationship type: Customer-Supplier. The Treatment Planning Context provides directives for radiation therapy, including prescribed dose, target site, and treatment intent. The Radiation Therapy Context translates these directives into a detailed dosimetric plan with target volumes, dose constraints, and fractionation schedules.

The radiation therapy context maintains full autonomy over technical planning decisions. The treatment planning context specifies what should be treated; the radiation therapy context determines how to deliver the prescribed treatment safely and effectively.

### Treatment Planning to Surgical Oncology

Relationship type: Customer-Supplier. The Treatment Planning Context recommends surgical interventions, specifying the type of procedure, timing relative to other modalities (neoadjuvant or adjuvant), and surgical goals. The Surgical Oncology Context translates these recommendations into detailed operative plans.

Surgical findings, particularly margin assessment results and lymph node status, flow back to the Diagnosis and Staging Context as pathological staging updates, which may trigger treatment plan revisions.

### Survivorship Care to All Treatment Contexts

Relationship type: Conformist (partial). The Survivorship Care Context conforms to the models of the treatment contexts to create accurate treatment summaries. It consumes completed treatment records from chemotherapy, radiation, and surgical contexts without imposing its own model on them.

The survivorship context assembles a comprehensive treatment history by subscribing to treatment completion events from each modality context. It then applies its own survivorship-specific rules for generating follow-up schedules and monitoring plans.

### External System Relationships

The oncology system interacts with several external systems through anti-corruption layers. Laboratory information systems provide test results to the Diagnosis and Staging Context. Pharmacy systems interface with the Chemotherapy Management Context for drug inventory and dispensing. Imaging systems provide diagnostic images to the Diagnosis and Staging Context and treatment planning images to the Radiation Therapy Context. Tumor registries receive case data from the Diagnosis and Staging Context for population-level reporting.

## Integration Patterns

The oncology context map uses several DDD integration patterns. Published Language is used extensively, with TNM staging, NCCN guidelines, and CTCAE grading serving as shared standards that reduce translation complexity between contexts. Open Host Service is used where contexts expose well-defined APIs for consuming their domain data. Anti-Corruption Layer is applied at boundaries with legacy external systems to prevent their models from contaminating oncology domain models.

## Conflict Resolution

When contexts disagree, the context map defines resolution procedures. If the chemotherapy context determines that a planned regimen is unsafe, the treatment planning context is notified and a revised tumor board review may be triggered. If surgical findings contradict the preoperative staging, the diagnosis and staging context updates the stage and the treatment planning context re-evaluates the treatment strategy.

## Maintaining the Context Map

The context map is a living document that must be updated as the oncology system evolves. Changes in clinical practice, new treatment modalities such as CAR-T cell therapy, and evolving integration requirements with external systems all necessitate context map revisions. Regular review sessions involving domain experts from each bounded context help maintain accuracy.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3: Context Maps.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7: Context Mapping.
