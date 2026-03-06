# Medical - Domain-Driven Design

This domain applies Domain-Driven Design (DDD) to medical practice, clinical care, diagnostics, pharmacy, insurance claims, and telemedicine. It is distinct from hospital management: this domain focuses on the clinical and medical practice perspective rather than hospital operations and administration.

## Bounded Contexts

- **Patient Records Context** - EHR, patient demographics, medical history, problem lists
- **Clinical Workflow Context** - Orders, clinical decision support, care plans, referrals
- **Diagnostics & Imaging Context** - Lab orders, results, radiology, pathology, imaging
- **Pharmacy & Medication Context** - Prescriptions, drug interactions, formulary, medication administration
- **Insurance & Claims Context** - Coverage verification, claims submission, adjudication, EOB
- **Telemedicine Context** - Virtual visits, remote monitoring, telehealth scheduling

## Ubiquitous Language

The shared vocabulary between developers and clinicians is defined in `ubiquitous-language.md`.

## Directory Structure

- `index.md` - Main documentation entry point
- `plan.md` - Project plan
- `tasks.md` - Task tracking
- `ubiquitous-language.md` - Ubiquitous language glossary
- `topics/` - Detailed topic documentation

## Conventions

- Every directory has `index.md` with a symlink `README.md` -> `index.md`
- No images, diagrams, web apps, front-end, or back-end code
- Comprehensive documentation with references and citations
- Ubiquitous language terms: one per line, one-sentence explanation

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
- Benson, Tim & Grieve, Grahame. "Principles of Health Interoperability: FHIR, HL7 and SNOMED CT." Springer, 2021.
