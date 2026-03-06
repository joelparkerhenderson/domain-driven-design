# Mast Cell Activation Syndrome Standards

## Overview

Domain-specific standards inform the design of value objects, published languages, and business rules within the MCAS domain model. These standards come from clinical guidelines, consensus statements, laboratory reference standards, and pharmacological guidelines. Understanding and incorporating these standards ensures that the domain model reflects clinical reality and supports evidence-based practice.

Standards in the MCAS domain are particularly important because the condition is relatively recently recognized, and diagnostic criteria continue to evolve through expert consensus processes. The domain model must accommodate current standards while being flexible enough to incorporate future revisions.

## Diagnostic Criteria Standards

### Consensus-1 Criteria (2012)

The first formal consensus criteria for MCAS were proposed by Valent et al. in 2012. These criteria require: episodic symptoms consistent with mast cell mediator release affecting two or more organ systems, a documented increase in a validated mast cell mediator during a symptomatic episode, and clinical response to medications targeting mast cell mediators or their effects.

The domain model's Consensus Criteria Result value object directly reflects this three-element framework. Each element is evaluated independently, and all three must be satisfied for a confirmed diagnosis.

### Consensus-2 Criteria (2020)

The Consensus-2 criteria, published by Afrin et al. in 2020, expanded the diagnostic framework to address a broader range of clinical presentations. These criteria maintain the core three-element structure but provide more detailed guidance on which mediator tests are considered validated, what constitutes a clinically significant elevation, and how treatment response should be assessed.

The domain model supports both Consensus-1 and Consensus-2 criteria evaluation, with the specific criteria version documented in each Diagnostic Workup aggregate.

### Proposed Diagnostic Algorithm (2019)

Valent et al. published a proposed diagnostic algorithm in 2019 that provides a step-by-step approach to evaluating patients with suspected MCAS. The algorithm includes guidance on initial screening, baseline mediator measurement, acute episode testing, and the sequence of diagnostic evaluations. The domain model's Diagnostic Workup aggregate reflects this algorithmic approach through its state transitions and required evaluation steps.

## Laboratory Standards

### Tryptase Measurement Standards

Serum tryptase is measured using standardized immunoassay methods. The clinically significant elevation threshold is defined as the acute level exceeding the patient's baseline multiplied by 1.2 plus 2 ng/mL. This formula is encoded in the Tryptase Level value object's comparison behavior.

Specimen collection standards require blood draw during or within four hours of a symptomatic episode for acute levels, and collection during an asymptomatic period for baseline levels. The domain model enforces these temporal constraints through the Mediator Panel entity.

### Histamine Metabolite Standards

Urine N-methylhistamine is measured over a 24-hour collection period or as a spot urine corrected for creatinine. Clinically significant elevation is defined as exceeding the upper limit of the laboratory's reference range. The domain model's Histamine Metabolite Level value object incorporates collection method and reference range comparison.

### Prostaglandin D2 Metabolite Standards

Urine 11-beta-prostaglandin F2-alpha is measured as a marker for prostaglandin D2 production. Specimen collection and handling requirements are strict, as the metabolite is temperature-sensitive. The domain model tracks collection conditions as part of the specimen metadata.

## Pharmacological Standards

### Antihistamine Dosing Guidelines

Standard dosing guidelines for H1 and H2 antihistamines provide the foundation for the Medication Protocol Context's dosing models. MCAS treatment frequently requires supratherapeutic doses, and the domain model tracks the clinical justification for dosing above standard guidelines.

### Mast Cell Stabilizer Guidelines

Cromolyn sodium dosing follows established gastroenterology guidelines for the oral formulation. Ketotifen dosing follows published titration protocols that start at low doses and increase gradually. The domain model's Titration Plan incorporates these published titration schedules as default templates.

### Compounding Standards

The United States Pharmacopeia (USP) Chapters 795 (non-sterile compounding) and 797 (sterile compounding) provide standards for compounded medication preparation. The domain model's Compounding Specification aggregate references these standards for quality assurance requirements.

## Clinical Assessment Standards

### Symptom Severity Scales

The domain model supports standardized severity scales including numeric rating scales (0-10), visual analog scales, and categorical scales (mild, moderate, severe). The choice of scale is documented in the Severity Score value object to ensure consistent interpretation.

### Quality of Life Instruments

The domain supports validated quality of life instruments including the SF-36 (Medical Outcomes Study 36-Item Short Form Survey), EQ-5D (EuroQol Five-Dimension), and disease-specific instruments developed for mast cell disorders. Each instrument has defined scoring algorithms encoded in the Outcomes Measurement Context.

### Functional Status Assessments

Functional status measurement follows established rehabilitation medicine standards for assessing physical function, cognitive function, social participation, and occupational capacity.

## Interoperability Standards

### HL7 FHIR

The MCAS domain's external integrations conform to HL7 FHIR standards for healthcare data exchange. Relevant FHIR resources include Patient, Practitioner, DiagnosticReport, Observation, MedicationRequest, CarePlan, and ServiceRequest. Anti-corruption layers translate between FHIR resources and domain models.

### LOINC

Laboratory Logical Observation Identifiers Names and Codes (LOINC) provide standardized identifiers for laboratory tests. The anti-corruption layer in the Diagnostic Assessment Context maps LOINC codes to domain-specific test identifiers.

### SNOMED CT

Systematized Nomenclature of Medicine Clinical Terms provides standardized clinical terminology. The domain model's ubiquitous language aligns with SNOMED CT concepts where applicable, facilitating interoperability with external clinical systems.

## Standards Governance

The MCAS domain model maintains a registry of all referenced standards, including version numbers and effective dates. When standards are updated, the domain model is reviewed for alignment, and necessary changes are propagated through the affected bounded contexts.

## References

- Valent, P. et al. "Definitions, Criteria and Global Classification of Mast Cell Disorders." International Archives of Allergy and Immunology, 2012.
- Afrin, L.B. et al. "Diagnosis of Mast Cell Activation Syndrome: A Global Consensus-2." Diagnosis, 2020.
- Valent, P. et al. "Proposed Diagnostic Algorithm for Patients with Suspected Mast Cell Activation Syndrome." Journal of Allergy and Clinical Immunology: In Practice, 2019.
- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
