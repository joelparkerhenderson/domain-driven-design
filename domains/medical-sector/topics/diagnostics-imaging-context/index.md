# Diagnostics & Imaging Context in Medical Practice

## Overview

The Diagnostics & Imaging Context is the bounded context responsible for managing laboratory testing, radiology studies, pathology analysis, and all forms of medical imaging. It receives orders from the Clinical Workflow context, manages specimen collection and imaging acquisition, processes results, and publishes findings back to the ordering provider. This context has its own specialized vocabulary around specimens, modalities, reporting standards, and result interpretation.

## Core Responsibilities

- **Lab Order Management**: Receiving, validating, and routing laboratory test orders to internal or external labs
- **Specimen Tracking**: Managing specimen collection, labeling, transport, and chain of custody
- **Lab Result Processing**: Recording test results with measured values, reference ranges, abnormal flags, and interpretive comments
- **Imaging Order Management**: Receiving and scheduling radiology and imaging study orders
- **Image Acquisition and Storage**: Managing DICOM image capture, archiving in PACS, and image lifecycle
- **Radiology Reporting**: Supporting radiologist interpretation with structured reports and findings
- **Pathology Reporting**: Managing tissue specimen analysis, histological findings, and pathologist reports
- **Critical Result Notification**: Identifying and escalating critical or panic-value results for immediate clinical attention

## Aggregates

### LabOrder Aggregate

- **Aggregate Root**: LabOrder (identified by OrderID)
- **Value Objects**: TestPanel (list of ordered tests with LOINC codes), SpecimenRequirement (type, volume, container), ClinicalIndication, Priority (routine, urgent, stat)
- **Invariants**: A lab order must have at least one test; priority must be valid; ordering provider must be identified

### LabResult Aggregate

- **Aggregate Root**: LabResult (identified by ResultID)
- **Value Objects**: LabResultValue (measured value, unit, reference range, abnormal flag), LOINCCode, PerformingLab, ResultStatus (preliminary, final, corrected)
- **Invariants**: A final result cannot be modified (only corrected with a new version); critical values must be flagged

### ImagingStudy Aggregate

- **Aggregate Root**: ImagingStudy (identified by StudyID, linked to DICOM Study Instance UID)
- **Value Objects**: Modality (X-ray, CT, MRI, ultrasound, PET, mammography), BodyRegion, ContrastUsed, DICOMMetadata
- **Invariants**: A study must have an associated patient and ordering provider; DICOM metadata must be valid

### PathologyReport Aggregate

- **Aggregate Root**: PathologyReport (identified by ReportID)
- **Value Objects**: SpecimenDescription, HistologicalFindings, DiagnosisSummary, Staging (TNM classification), Margin status
- **Invariants**: A finalized pathology report is immutable; all specimen identifiers must match the accessioned specimen

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| LabOrderReceived | Order accepted from clinical context | Lab workflow |
| SpecimenCollected | Sample obtained from patient | Lab processing |
| LabResultAvailable | Test completed with results | Clinical Workflow, Patient Records |
| CriticalResultFlagged | Panic/critical value detected | Clinical Workflow (urgent notification) |
| ImagingStudyCompleted | Imaging performed and stored | Clinical Workflow, Patient Records |
| RadiologyReportFinalized | Radiologist completes report | Clinical Workflow, Patient Records |
| PathologyReportFinalized | Pathologist completes report | Clinical Workflow, Patient Records |

## Integration with Other Contexts

### Diagnostics <- Clinical Workflow (Customer-Supplier)

The Clinical Workflow context places lab and imaging orders. The Diagnostics context receives these orders and manages them through its internal workflow.

### Diagnostics -> Clinical Workflow (Customer-Supplier)

Results flow back to the clinical context via LabResultAvailable, ImagingStudyCompleted, and report finalization events. The clinical context incorporates results into the encounter and patient record.

### Diagnostics -> Patient Records (Customer-Supplier)

Finalized results are pushed to the Patient Records context for longitudinal record keeping.

### Diagnostics <-> External Reference Labs (ACL)

External reference laboratories communicate via HL7 v2 ORM (orders) and ORU (results) messages. An anti-corruption layer translates between HL7 v2 format and the internal domain model.

### Diagnostics <-> PACS (ACL)

Picture Archiving and Communication Systems use DICOM protocols. An ACL manages DICOM C-STORE, C-FIND, and WADO-RS interactions while keeping the internal model clean.

## Medical Standards in This Context

- **LOINC**: Used to identify lab test types and observation codes universally
- **DICOM**: Used for imaging acquisition, storage, and communication
- **HL7 v2**: Used for interfacing with external laboratory information systems
- **SNOMED CT**: Used for anatomical and clinical finding terminology in reports

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Diagnostics & Imaging is one of six medical bounded contexts
- [Clinical Workflow Context](../clinical-workflow-context/) -- Upstream source of orders, downstream consumer of results
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate HL7 v2 and DICOM protocols
- [Value Objects](../value-objects/) -- LOINCCode, LabResultValue, ImagingModality are key value objects
- [Medical Standards](../medical-standards/) -- LOINC, DICOM, and HL7 are foundational standards
- [Domain Events](../domain-events/) -- Result availability events drive clinical workflows

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- HL7 International. "FHIR R4 DiagnosticReport Resource." https://hl7.org/fhir/diagnosticreport.html
- HL7 International. "FHIR R4 ImagingStudy Resource." https://hl7.org/fhir/imagingstudy.html
- DICOM Standards Committee. "DICOM Standard." https://www.dicomstandard.org/
- Regenstrief Institute. "LOINC Users' Guide." https://loinc.org/
- Pantanowitz, Liron. "Digital Pathology." ASCP Press, 2017.
