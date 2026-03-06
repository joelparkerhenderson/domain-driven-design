# Medical DDD - Project Plan

## Goal

Create comprehensive Domain-Driven Design documentation for a medical practice domain, covering clinical workflows, diagnostics, pharmacy, insurance, and telemedicine through strategic design, tactical design, bounded contexts, and cross-cutting concerns.

## Phases

### Phase 1: Scaffolding

- [x] Create CLAUDE.md - Domain-specific instructions and conventions
- [x] Create plan.md - This project plan
- [x] Create tasks.md - Task tracking checklist
- [x] Create ubiquitous-language.md - Ubiquitous language glossary
- [x] Create index.md with README.md symlink
- [x] Create topics/index.md with README.md symlink

### Phase 2: Strategic Design Topics

- [x] Document strategic design principles applied to medical practice
- [x] Document bounded contexts and their boundaries
- [x] Document context map showing relationships between contexts
- [x] Document ubiquitous language conventions
- [x] Document subdomain identification (core, supporting, generic)
- [x] Document anti-corruption layers for external system integration

### Phase 3: Tactical Design Topics

- [x] Document aggregates (PatientRecord + ProblemList + MedicalHistory)
- [x] Document entities (Patient, Encounter, Prescription, Claim)
- [x] Document value objects (ICD10Code, CPTCode, Dosage, VitalSigns)
- [x] Document domain events (PrescriptionIssued, ClaimSubmitted, LabResultReceived)
- [x] Document repositories for aggregate root persistence
- [x] Document domain services spanning multiple aggregates

### Phase 4: Bounded Context Topics

- [x] Document Patient Records Context
- [x] Document Clinical Workflow Context
- [x] Document Diagnostics & Imaging Context
- [x] Document Pharmacy & Medication Context
- [x] Document Insurance & Claims Context
- [x] Document Telemedicine Context

### Phase 5: Cross-Cutting Concerns

- [x] Document event-driven architecture patterns
- [x] Document integration patterns between contexts
- [x] Document medical standards (HL7, FHIR, DICOM, ICD-10, SNOMED CT, LOINC)
- [x] Document regulatory compliance (HIPAA, HITECH, FDA 21 CFR Part 11)

### Phase 6: Review and Finalize

- [x] Verify all symlinks and file structure
- [x] Cross-reference topics for consistency
- [x] Update tasks.md to reflect completion

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett & Tune. "Patterns, Principles, and Practices of DDD." Wrox, 2015.
