# Ubiquitous Language in the Pulmonolgy Domain

## Overview

Ubiquitous language is the shared vocabulary that domain experts, developers, and stakeholders use when discussing the pulmonolgy domain. Every term in the ubiquitous language has a precise, agreed-upon meaning within the context where it is used. The language appears in conversations, documentation, code, and tests, ensuring that communication is unambiguous and that the software model faithfully represents the domain.

In pulmonolgy, the ubiquitous language draws from respiratory medicine terminology while adding precision required for software modeling. Clinical terms like spirometry, exacerbation, and ventilator weaning carry specific meanings that must be consistently understood by both clinicians providing domain expertise and developers building the system.

## Language Boundaries

The ubiquitous language is scoped to bounded contexts. A term may have different meanings in different contexts, and this is expected and healthy. For example:

- **Compliance**: In the Sleep Medicine Context, compliance refers to a patient's adherence to CPAP therapy (measured as hours of nightly use). In the Regulatory Compliance cross-cutting concern, compliance refers to adherence to legal and governance requirements such as HIPAA.

- **Assessment**: In the Pulmonary Assessment Context, assessment means the initial clinical evaluation of a patient including symptom history, physical examination, and pulmonary function testing. In the Pulmonary Rehabilitation Context, assessment means the evaluation of a patient's functional capacity through standardized exercise tests.

- **Protocol**: In the Critical Care Context, protocol refers to a structured clinical algorithm for ventilator management or weaning. In the Chronic Disease Management Context, protocol refers to a disease-specific treatment guideline such as GOLD for COPD.

These distinctions are not inconsistencies to be resolved. They reflect the genuine differences in how domain experts in each area use these terms. Strategic design preserves these differences by maintaining separate models within separate bounded contexts.

## Governance

The ubiquitous language is governed through several practices:

- **Domain Expert Collaboration**: Terms are defined in partnership with pulmonologists, respiratory therapists, sleep technologists, and other domain experts. Developers do not invent domain terms; they learn and adopt the language used by clinicians.

- **Glossary Maintenance**: The ubiquitous-language.md file at the domain root serves as the canonical glossary. Each term includes a one-sentence definition that has been validated by domain experts. The glossary is updated whenever new concepts are introduced or existing definitions are refined.

- **Context-Specific Supplements**: Each bounded context may maintain supplementary terminology that is meaningful only within that context. These supplements extend the domain-level glossary without contradicting it.

- **Review Cycles**: The ubiquitous language is reviewed periodically with domain experts to ensure it remains accurate as clinical practice evolves. New clinical guidelines, updated classification systems, and emerging terminology are incorporated through this review process.

## Key Terms Across Contexts

Several categories of terms form the foundation of the pulmonolgy ubiquitous language:

### Clinical Measurement Terms
Spirometry, FEV1, FVC, DLCO, oxygen saturation (SpO2), arterial blood gas (ABG), peak expiratory flow (PEF), apnea-hypopnea index (AHI), and six-minute walk test (6MWT). These terms represent quantifiable clinical measurements with standardized units and reference ranges.

### Disease Classification Terms
Obstructive lung disease, restrictive lung disease, COPD, asthma, interstitial lung disease (ILD), pulmonary hypertension, cystic fibrosis, ARDS, and obstructive sleep apnea (OSA). Each disease term carries specific diagnostic criteria and severity classification scales.

### Procedural Terms
Bronchoscopy, thoracentesis, lung biopsy, polysomnography, mechanical ventilation, CPAP, BiPAP, and ventilator weaning. These terms describe clinical interventions with defined workflows, safety requirements, and outcome measures.

### Care Management Terms
Treatment plan, exacerbation, bronchodilator reversibility, pulmonary rehabilitation, exercise training, breathing techniques, and self-management. These terms describe the ongoing management of chronic respiratory conditions.

## Language in Practice

The ubiquitous language manifests in several practical ways:

- **Model Naming**: Aggregate roots, entities, value objects, and domain events are named using terms from the ubiquitous language. An Exacerbation entity in the Chronic Disease Management Context uses the clinical term directly.

- **Event Naming**: Domain events follow the pattern of past-tense clinical actions: AssessmentCompleted, DiagnosticResultAvailable, ExacerbationDetected, WeaningProtocolInitiated.

- **Service Naming**: Domain services describe clinical operations: SeverityClassificationService, VentilatorWeaningReadinessService, RehabilitationOutcomeService.

- **Validation Rules**: Business rules and validation logic use domain language. A rule might state "FEV1/FVC ratio below 0.70 post-bronchodilator confirms obstructive ventilatory defect" using the precise clinical terminology.

## Evolution

The ubiquitous language evolves as the domain evolves. New clinical guidelines may introduce updated terminology, deprecated classification systems may be replaced, and emerging subspecialties may introduce new concepts. The language governance process ensures these changes are incorporated systematically, with full traceability from clinical source to model implementation.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 2 on ubiquitous language and its role in bridging domain experts and developers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 1 on developing a ubiquitous language within bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 2 on discovering and formalizing ubiquitous language.
