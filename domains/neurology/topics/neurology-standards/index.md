# Neurology Standards - Neurology Domain

## Overview

The neurology domain model is informed by a set of clinical, technical, and interoperability standards that govern how neurological data is captured, communicated, and interpreted. These standards shape value object design, published languages, and anti-corruption layer translations throughout the domain.

Adherence to established standards ensures that the neurology domain model is clinically valid, interoperable with external systems, and consistent with accepted medical practice.

## Clinical Practice Standards

### American Academy of Neurology (AAN)

The AAN publishes evidence-based clinical practice guidelines that inform the domain model:

- Practice parameters for diagnostic evaluation of neurological conditions.
- Quality measures for neurological care delivery.
- Clinical pathway recommendations that shape workflow models within bounded contexts.
- Position statements on emerging therapies that influence treatment protocol design.

Impact on the domain model:
- Clinical Assessment Context examination components align with AAN-recommended evaluation standards.
- Treatment protocols in Neurodegenerative Disease and Epilepsy Management contexts reflect AAN guideline recommendations.
- Quality metrics embedded in the domain model reference AAN quality measures.

### International League Against Epilepsy (ILAE)

The ILAE provides the authoritative framework for epilepsy classification:

- 2017 Operational Classification of Seizure Types: defines the seizure classification taxonomy used by the SeizureClassification value object.
- 2017 Classification of the Epilepsies: defines epilepsy syndrome classification.
- Definition of drug-resistant epilepsy (Kwan et al., 2010): establishes criteria used in the DrugResistanceDetermined domain event.
- Guidelines for pre-surgical evaluation: inform the surgical candidacy workflow in the Epilepsy Management Context.

Impact on the domain model:
- SeizureClassification value objects strictly conform to the ILAE hierarchy.
- AED trial documentation follows ILAE definitions for adequate drug trials.
- Epilepsy syndrome classification uses ILAE terminology.

### Movement Disorder Society (MDS)

The MDS provides standards for movement disorder assessment:

- MDS-UPDRS: the standardized rating scale for Parkinson's disease modeled as the UPDRSScore value object.
- MDS Diagnostic Criteria for Parkinson's Disease (Postuma et al., 2015): informs the diagnostic pathway in the Neurodegenerative Disease Context.
- Hoehn and Yahr staging: modeled as a DiseaseStage value object.

### NIA-AA Research Framework

The National Institute on Aging and Alzheimer's Association (NIA-AA) framework:

- AT(N) biomarker classification: Amyloid (A), Tau (T), Neurodegeneration (N).
- Defines the biomarker measurement value objects in the Neurodegenerative Disease Context.
- Informs clinical trial eligibility criteria for Alzheimer's disease research.

### McDonald Criteria for Multiple Sclerosis

The 2017 revised McDonald Criteria (Thompson et al., 2018):

- Diagnostic criteria for MS based on dissemination in space and time.
- Defines how MRI lesion data from the Neuroimaging Context is interpreted in the Neurodegenerative Disease Context.
- Distinguishes relapsing-remitting from progressive MS subtypes.

## Technical Interoperability Standards

### DICOM (Digital Imaging and Communications in Medicine)

DICOM governs neuroimaging data management:

- Image Storage: defines how MRI, CT, PET, and other neuroimaging data is stored and archived.
- Networking: C-FIND, C-MOVE, C-STORE, C-ECHO protocols for imaging workflow.
- Structured Reporting: SR templates for machine-readable imaging reports.
- Worklist Management: modality worklist for imaging acquisition scheduling.
- Web Access (WADO): HTTP-based access to imaging data.

Impact on the domain model:
- The Neuroimaging Context's ImagingStudy aggregate uses DICOM Study Instance UID as a key identifier.
- The open host service exposes DICOM-compliant interfaces.
- Imaging value objects (ImagingModality, ImagingFinding) align with DICOM terminology.

### HL7 FHIR (Fast Healthcare Interoperability Resources)

FHIR R4 provides the published language for healthcare data exchange:

- Patient: patient demographics and identifiers.
- Encounter: clinical encounter documentation.
- Condition: neurological diagnoses.
- Observation: clinical findings, vital signs, assessment scores.
- DiagnosticReport: imaging and laboratory reports.
- MedicationRequest: medication orders including AEDs and DMTs.
- CarePlan: rehabilitation care plans.

Impact on the domain model:
- Anti-corruption layers translate between FHIR resources and internal domain objects.
- Published language for external system integration uses FHIR profiles.
- Neurology-specific FHIR extensions may be defined for concepts not in the base standard.

### Clinical Terminology Standards

**ICD-10/ICD-11**: diagnostic codes for neurological conditions (G00-G99 chapter).
**CPT**: procedure codes for neurological examinations, imaging, electrodiagnostic studies.
**RxNorm**: medication coding for AEDs, DMTs, and other neurological medications.
**LOINC**: laboratory observation codes for biomarkers, CSF analysis, genetic tests.
**SNOMED CT**: comprehensive clinical terminology for neurological concepts.

### ACMG Guidelines

The American College of Medical Genetics and Genomics provides:

- Standards for variant classification: pathogenic, likely pathogenic, variant of uncertain significance (VUS), likely benign, benign.
- Used by the GeneticVariant value object in the Neuromuscular Disorders Context.
- Informs the GeneticVariantClassificationService domain service.

## Electrodiagnostic Standards

### AANEM (American Association of Neuromuscular and Electrodiagnostic Medicine)

- Standards for nerve conduction study performance and interpretation.
- Normal value reference ranges by age and temperature.
- Criteria for demyelination vs. axonal pathology.
- Standards for repetitive nerve stimulation and single fiber EMG.

Impact on the domain model:
- NerveConduction value objects include normal reference ranges from AANEM standards.
- Electrodiagnostic interpretation services apply AANEM criteria for pathology classification.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Academy of Neurology. Clinical Practice Guidelines. https://www.aan.com
- International League Against Epilepsy. ILAE Classification and Terminology.
- DICOM Standards Committee. DICOM Standard. https://www.dicomstandard.org
- HL7 International. FHIR R4 Specification. https://hl7.org/fhir
