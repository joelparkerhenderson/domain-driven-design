# Respiratory Diagnostics Context

## Overview

The Respiratory Diagnostics Context handles advanced diagnostic procedures within the pulmonolgy domain. It encapsulates the workflows for spirometry interpretation, diffusing capacity (DLCO) testing, bronchoscopy, thoracentesis, and lung biopsy. This context provides diagnostic confirmation and detailed disease characterization through procedural and laboratory workflows that require specialized clinical expertise and safety protocols.

As a core subdomain, the Respiratory Diagnostics Context receives the highest modeling investment because diagnostic accuracy and procedural excellence are primary clinical differentiators. The complexity of multi-test correlation, specimen chain-of-custody management, and diagnostic classification logic demands deep domain modeling.

## Clinical Scope

The Respiratory Diagnostics Context covers the following clinical activities:

- **Advanced Spirometry Interpretation**: Beyond basic FEV1/FVC assessment, this includes flow-volume loop pattern analysis, bronchoprovocation testing, and pre/post bronchodilator response characterization.

- **Diffusing Capacity Testing**: DLCO measurement, adjustment for hemoglobin and altitude, and interpretation in the context of concurrent spirometric findings to characterize gas exchange impairment.

- **Bronchoscopy**: Flexible and rigid bronchoscopy procedures including bronchoalveolar lavage (BAL), transbronchial biopsy, endobronchial ultrasound (EBUS), and navigational bronchoscopy. Encompasses pre-procedure assessment, procedural execution, specimen collection, and post-procedure monitoring.

- **Thoracentesis**: Diagnostic and therapeutic pleural fluid drainage. Includes fluid analysis interpretation using Light's criteria for transudative versus exudative classification.

- **Lung Biopsy**: Transbronchial biopsy, CT-guided percutaneous biopsy, and surgical lung biopsy coordination. Manages specimen collection, handling requirements, and pathology result integration.

## Aggregates

**Diagnostic Procedure Aggregate**: The primary aggregate managing procedural workflows. Root entity is DiagnosticProcedure, containing ProceduralStep entities, SpecimenCollection records, and ComplicationEvent value objects. Enforces the invariant that informed consent must be documented before procedure initiation and that all specimens are linked to their originating procedure.

**Diagnostic Result Aggregate**: Manages the compilation and finalization of diagnostic results. Root entity is DiagnosticResult, containing individual test interpretations, pathology findings, and clinical correlation notes. Enforces the invariant that results cannot be finalized without a reviewing clinician's attestation.

## Key Entities

- **Diagnostic Procedure**: Tracks the lifecycle from order through completion, including scheduling, consent, execution, and result documentation.
- **Specimen**: Maintains chain-of-custody integrity from collection through pathology processing.
- **Proceduralist**: Tracks physician privileges, procedure volume, and credentialing for quality assurance.

## Key Value Objects

- **DLCOResult**: Diffusing capacity measurement with adjustments and percent predicted.
- **BronchoscopyFinding**: Anatomical location, finding type, and visual description.
- **BiopsyResult**: Tissue type, histological findings, and diagnostic classification.
- **PleuraFluidAnalysis**: Fluid characteristics and Light's criteria classification.

## Domain Events Published

- **DiagnosticProcedureCompleted**: Signals procedure completion with specimen identifiers.
- **DiagnosticResultAvailable**: Signals finalized diagnostic results for clinical review.
- **ProcedureComplicationOccurred**: Signals procedural complications requiring management.

## Domain Services

- **DiagnosticIndicationService**: Evaluates clinical data to determine which procedures are indicated.
- **SpecimenTrackingService**: Coordinates specimen chain-of-custody from collection through processing.
- **DiagnosticCorrelationService**: Integrates results across multiple diagnostic modalities.

## Context Relationships

The Respiratory Diagnostics Context is downstream of the Pulmonary Assessment Context via a customer-supplier relationship. Assessment findings and diagnostic orders flow from the assessment context and are consumed as inputs for determining diagnostic procedure selection and prioritization.

The context maintains a partnership relationship with the Chronic Disease Management Context. Diagnostic results directly inform disease classification and treatment plan adjustments, and both teams coordinate model changes through a shared published language for diagnostic result communication.

When procedure complications are severe, the context may trigger involvement from the Critical Care Context through ProcedureComplicationOccurred events.

## Business Rules

- No diagnostic procedure may begin without documented informed consent and pre-procedure safety assessment.
- Specimens must be processed within defined time and temperature limits specific to the specimen type and intended analysis.
- Bronchoscopy findings must be documented using standardized anatomical nomenclature with segment-level specificity.
- Diagnostic results require attestation by a qualified interpreting physician before publication to downstream contexts.
- Pleural fluid analysis must apply Light's criteria and document the transudative/exudative classification with supporting laboratory values.
- Procedure complication rates are tracked per proceduralist for quality monitoring, with threshold alerts for rates exceeding benchmarks.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Thoracic Society. Diagnostic Standards and Classification of Tuberculosis in Adults and Children. *American Journal of Respiratory and Critical Care Medicine*, 2000.
- Du Rand, I.A., et al. British Thoracic Society Guideline for Diagnostic Flexible Bronchoscopy in Adults. *Thorax*, 2013.
