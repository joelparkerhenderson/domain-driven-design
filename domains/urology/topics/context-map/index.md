# Context Map for the Urology Domain

## Overview

A context map provides a visual and conceptual representation of the relationships between bounded contexts. In the urology domain, the context map documents how the six bounded contexts interact, what information flows between them, and what integration patterns govern those interactions. The context map is essential for understanding the system as a whole and for identifying where translation, coordination, and conflict resolution are needed.

## Context Relationships

### Clinical Assessment to Treatment Contexts

The Clinical Assessment Context serves as the primary upstream context for all treatment-oriented contexts. It publishes diagnostic profiles that are consumed by the Oncologic Urology Context, Stone Disease Context, Incontinence Management Context, and Male Reproductive Health Context. This is a customer-supplier relationship where the treatment contexts are customers of the diagnostic data produced by Clinical Assessment.

### Clinical Assessment to Surgical Management

The Clinical Assessment Context and Surgical Management Context share a partnership relationship. Clinical Assessment provides the diagnostic workup that informs surgical planning, while Surgical Management feeds operative findings back to Clinical Assessment, which may revise the diagnostic profile. Both teams must coordinate when changes affect the shared interface.

### Oncologic Urology to Surgical Management

The Oncologic Urology Context acts as a customer to the Surgical Management Context's supplier role for oncologic procedures. The Oncologic context defines what surgery is needed (radical prostatectomy, partial nephrectomy, radical cystectomy), and the Surgical Management context executes the procedure and returns operative outcomes. The Oncologic context translates surgical findings into its staging and surveillance models.

### Stone Disease to Surgical Management

The Stone Disease Context and Surgical Management Context have a customer-supplier relationship for procedural stone interventions (ESWL, ureteroscopy, PCNL). The Stone Disease context determines the treatment plan based on stone characteristics, and the Surgical Management context manages the procedural execution.

### Incontinence Management to Surgical Management

The Incontinence Management Context consumes surgical capabilities for procedures such as mid-urethral slings, artificial urinary sphincter placement, and sacral neuromodulation implantation. When conservative management fails, the Incontinence context refers patients to the Surgical Management context for procedural intervention.

### Male Reproductive Health to Surgical Management

The Male Reproductive Health Context utilizes the Surgical Management Context for microsurgical procedures including varicocelectomy, vasectomy, vasectomy reversal, and penile prosthesis implantation. The reproductive context owns the clinical decision to proceed, while the surgical context owns the procedural protocol.

## External System Integrations

### Pathology Systems

The Oncologic Urology Context and Stone Disease Context both integrate with external pathology systems. The Oncologic context receives biopsy results, Gleason grades, and margin status reports. The Stone Disease context receives stone composition analysis results. Both use anti-corruption layers to translate pathology data into their own domain models.

### Radiology Systems

The Clinical Assessment Context integrates with radiology information systems to receive imaging reports and structured findings. CT urograms, renal ultrasounds, MRI prostate studies, and nuclear medicine scans are translated from radiology terminology into the Clinical Assessment context's diagnostic model.

### EHR Systems

All six bounded contexts integrate with the electronic health record system as a generic subdomain. The EHR provides patient demographics, medication lists, allergy information, and clinical notes. Each context maintains an anti-corruption layer to translate EHR data into its own model.

### Pharmacy Systems

The Incontinence Management Context and Male Reproductive Health Context integrate with pharmacy systems for medication management (antimuscarinics, alpha-blockers, PDE5 inhibitors, testosterone formulations). The Stone Disease Context integrates for metabolic prevention medications (thiazides, potassium citrate, allopurinol).

## Integration Patterns

The urology context map employs several DDD integration patterns. Published Language is used for standardized clinical data exchange (HL7 FHIR resources). Open Host Service is used by the Clinical Assessment Context to expose diagnostic data to multiple consumers. Shared Kernel exists between the Oncologic Urology Context and the Surgical Management Context for shared cancer staging terminology. Conformist relationships exist with external standards bodies (AUA, EAU, AJCC) whose models the urology domain adopts without modification.

## Conflict Resolution

When bounded contexts disagree on terminology or model structure, the context map provides the framework for resolution. The upstream context's model takes precedence for data it produces, but downstream contexts are free to translate that data into their own representations. Domain events serve as the primary mechanism for loose coupling between contexts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8.
- Brandolini, Alberto. Introducing EventStorming. Leanpub, 2021.
