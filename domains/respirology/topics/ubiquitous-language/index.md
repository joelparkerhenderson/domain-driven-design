# Ubiquitous Language in the Respirology Domain

## Overview

Ubiquitous language is the shared vocabulary used by domain experts, developers, and stakeholders when discussing the respirology domain. Every term in the ubiquitous language carries a precise meaning agreed upon by all participants. The language appears in conversations, documentation, code, and test cases. When the ubiquitous language is consistent and well-maintained, it eliminates translation errors between clinical staff and development teams, ensuring that the software model faithfully represents clinical reality.

## Purpose

In respirology, precision of language is clinically critical. The difference between "dyspnea" and "respiratory distress" carries implications for severity assessment. The distinction between "FEV1" and "FVC" determines diagnostic classification. The ubiquitous language codifies these distinctions so that they are preserved in the domain model.

The ubiquitous language serves several functions:

- **Alignment**: Pulmonologists, respiratory therapists, nurses, and developers use the same terms with the same meanings.
- **Precision**: Ambiguous terms are resolved. When "assessment" is used in the Clinical Assessment Context, it means a structured respiratory examination, not a generic evaluation.
- **Documentation**: The language provides a glossary that new team members can use to onboard quickly.
- **Validation**: Domain experts can review code and models using familiar clinical terminology, identifying errors that would be invisible to non-specialists.

## Context-Specific Language

The ubiquitous language is scoped to bounded contexts. Some terms are shared across the entire respirology domain, while others have context-specific meanings:

### Domain-Wide Terms

Terms such as "patient," "respiratory therapist," "pulmonologist," and "exacerbation" carry consistent meanings across all bounded contexts. These terms are defined in the domain-level glossary and should not be redefined within individual contexts.

### Clinical Assessment Terms

Within the Clinical Assessment Context, terms include "respiratory examination," "auscultation," "symptom score," "mMRC grade," "CAT score," "Borg scale rating," and "risk stratification." These terms describe the specific activities and outputs of clinical assessment.

### Respiratory Diagnostics Terms

Within the Respiratory Diagnostics Context, terms include "spirometry," "FEV1," "FVC," "FEV1/FVC ratio," "plethysmography," "DLCO," "bronchoscopy," "bronchoalveolar lavage," "sputum culture," and "diagnostic report." These terms describe diagnostic procedures and their outputs.

### Airway Management Terms

Within the Airway Management Context, terms include "intubation," "extubation," "tracheostomy," "decannulation," "airway clearance," "chest physiotherapy," "postural drainage," "bronchodilator," "SABA," "LABA," "SAMA," and "LAMA." These terms describe airway procedures and therapies.

### Chronic Disease Management Terms

Within the Chronic Disease Management Context, terms include "care plan," "action plan," "GOLD classification," "GOLD stage," "exacerbation record," "ILD monitoring protocol," "bronchiectasis management plan," and "sarcoidosis surveillance." These terms describe long-term disease management activities.

### Ventilatory Support Terms

Within the Ventilatory Support Context, terms include "non-invasive ventilation," "BiPAP," "CPAP," "mechanical ventilation," "tidal volume," "PEEP," "FiO2," "ventilation mode," "pressure support," "volume control," "weaning protocol," "spontaneous breathing trial," and "high-flow nasal therapy." These terms describe ventilation configuration and management.

### Pulmonary Rehabilitation Terms

Within the Pulmonary Rehabilitation Context, terms include "rehabilitation program," "exercise prescription," "breathing retraining," "pursed-lip breathing," "diaphragmatic breathing," "six-minute walk test," "functional assessment," "patient education module," and "discharge plan." These terms describe rehabilitation activities and outcomes.

## Language Governance

The ubiquitous language is maintained through:

- **Glossary Reviews**: Periodic reviews with domain experts to ensure terms remain current with clinical practice.
- **Model Alignment**: When the domain model changes, the ubiquitous language is updated simultaneously.
- **Conflict Resolution**: When two contexts use the same term with different meanings, the conflict is documented and the term is qualified with its context (e.g., "assessment" becomes "respiratory assessment" or "functional assessment" depending on context).
- **New Term Introduction**: When domain experts introduce new clinical concepts, corresponding terms are added to the ubiquitous language before being incorporated into the model.

## Language and Code

The ubiquitous language should be reflected directly in code. Class names, method names, variable names, and event names should use terms from the ubiquitous language. For example, a class representing a COPD care plan should be named `COPDCarePlan`, not `PatientPlanType3`. Method names should reflect domain actions: `assessDyspneaSeverity()`, not `calculateScore()`.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 2 on ubiquitous language.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 1 on the importance of ubiquitous language.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 2 on discovering domain knowledge through language.
