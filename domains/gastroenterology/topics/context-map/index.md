# Context Map in Gastroenterology

## Overview

A context map is a visual and conceptual representation of the relationships and
integration patterns between bounded contexts. In the gastroenterology domain, the
context map describes how six bounded contexts communicate, which contexts are upstream
or downstream, and what integration strategies govern their interactions.

## Context Relationships

### Clinical Assessment as Upstream Provider

The Clinical Assessment Context is the primary upstream context. Nearly all clinical
workflows begin with patient assessment. When a clinical assessment identifies a need
for endoscopy, IBD management, hepatology consultation, motility evaluation, or
nutritional intervention, the Clinical Assessment Context publishes domain events
that downstream contexts consume. This is a customer-supplier relationship where the
assessment context defines the data contract for referrals.

### Endoscopic Procedures and Inflammatory Disease

The Endoscopic Procedures Context has a partnership relationship with the Inflammatory
Disease Context. Colonoscopy findings directly inform IBD disease activity scoring, and
IBD management decisions often trigger surveillance colonoscopy orders. Both teams
collaborate on the shared language for endoscopic disease severity. Pathology results
from endoscopic biopsies flow from the Endoscopic Procedures Context to the
Inflammatory Disease Context through published language.

### Endoscopic Procedures and Hepatology

ERCP and EUS procedures serve hepatology needs for biliary evaluation. The Endoscopic
Procedures Context provides procedural findings to the Hepatology Context through a
customer-supplier relationship. The Hepatology Context consumes these results to inform
liver disease staging and treatment decisions.

### Clinical Assessment and Motility Disorders

Patients with dysphagia, GERD symptoms, or functional complaints are referred from
Clinical Assessment to the Motility Disorders Context. The assessment context provides
symptom documentation and preliminary evaluation. The Motility Disorders Context
performs specialized testing and returns diagnostic conclusions.

### Clinical Assessment and Nutrition Management

Nutritional deficiencies, unexplained weight loss, or celiac disease suspicion
identified during clinical assessment trigger referrals to the Nutrition Management
Context. The assessment context provides anthropometric and biochemical data. The
Nutrition Management Context develops and tracks nutritional interventions.

### Hepatology and Nutrition Management

Patients with cirrhosis frequently require nutritional optimization. The Hepatology
Context communicates liver disease severity and dietary restrictions to the Nutrition
Management Context. This is a customer-supplier relationship where Hepatology defines
nutritional requirements and Nutrition Management fulfills them.

## Integration Patterns on the Map

### Shared Kernel

A small shared kernel exists for core identifiers: PatientId, EncounterId, and
ProviderId. These identifiers are consistent across all bounded contexts, enabling
correlation of records. The shared kernel is deliberately minimal to avoid tight
coupling.

### Published Language

Contexts communicate through published language contracts. The Endoscopic Procedures
Context publishes procedure reports in a standardized format. The Inflammatory Disease
Context publishes disease activity assessments. These contracts use clinical terminology
agreed upon by all participating contexts.

### Anti-Corruption Layers

External systems (EHR, lab information systems, pharmacy systems, radiology PACS)
integrate through anti-corruption layers at the domain boundary. Each bounded context
that needs external data translates incoming information into its own domain model.

### Conformist Relationships

The Nutrition Management Context acts as a conformist to the Clinical Assessment Context
for laboratory values. Rather than maintaining its own model of lab results, it adopts
the assessment context's representation, since lab values are authoritative and do not
require reinterpretation for nutritional purposes.

## Map Dynamics

The context map is not static. As gastroenterology practice evolves, relationships may
shift. New therapeutic modalities may create new integration points. Changes in clinical
guidelines may alter which context is authoritative for certain decisions. The context
map should be revisited periodically to reflect these changes.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Context Map.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3: Context Maps.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7: Context Mapping.
