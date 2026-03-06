# Entities in the Dermatology Domain

Entities are domain objects defined by their unique identity rather than their attributes. An entity persists over time and its attributes may change, but its identity remains constant. In the dermatology domain, entities represent clinical objects that have distinct lifecycles, evolve through various states, and must be individually tracked and referenced. As Evans (2003) describes, the distinction between entities and value objects is fundamental to domain modeling.

## Characteristics of Dermatology Entities

Dermatology entities share several characteristics. Each has a unique identifier that distinguishes it from all other instances of the same type. Each has a lifecycle that spans creation, modification, and potentially archival or closure. Each carries state that changes over time as clinical events occur. And each must be individually retrievable and referenceable across the system.

## Clinical Assessment Context Entities

### Skin Examination

A Skin Examination entity represents a single clinical encounter where a dermatologist evaluates a patient's skin. It is identified by a unique examination identifier and carries the examination date, examining provider, examination type (full-body, focused, or follow-up), and overall clinical impression. The entity transitions through states: scheduled, in-progress, documented, and finalized. Its attributes change as findings are added during the examination.

### Lesion Finding

A Lesion Finding entity represents a single lesion identified during a skin examination. It is identified by a unique finding identifier within the context of its parent examination. The entity carries the lesion's anatomical location, size measurements, morphological characteristics, and dermoscopic observations. A Lesion Finding evolves as additional observations are recorded, and it may trigger the creation of a Lesion Record in the Lesion Management Context.

## Lesion Management Context Entities

### Lesion Record

A Lesion Record entity tracks a single lesion through its complete lifecycle. It is identified by a unique lesion identifier and maintains a history of all assessments, biopsies, diagnoses, and treatments associated with that lesion. The entity transitions through states: identified, under-observation, biopsy-pending, biopsy-completed, diagnosed, treatment-planned, treated, in-follow-up, resolved, or recurred. Each state transition is significant and may trigger domain events.

### Biopsy Decision

A Biopsy Decision entity captures the clinical reasoning behind the decision to biopsy a specific lesion. It is identified by a unique decision identifier and carries the indication for biopsy, selected biopsy type (shave, punch, incisional, or excisional), target location, and decision rationale. The entity has a lifecycle: proposed, approved, scheduled, performed, and completed.

### Excision Plan

An Excision Plan entity defines the surgical plan for removing a lesion. It carries the planned margin width (based on diagnosis and clinical guidelines), the excision technique, and the reconstruction approach. The entity evolves from draft through approval to execution, and its attributes are refined as the diagnosis is confirmed and surgical planning progresses.

## Procedure Management Context Entities

### Procedure Session

A Procedure Session entity represents a single procedural encounter. It is identified by a unique session identifier and carries the procedure type, performing provider, procedure date, and session-specific parameters. The entity transitions through states: scheduled, prepared, in-progress, completed, and documented. It accumulates data during execution as individual steps are performed and recorded.

### Mohs Stage

A Mohs Stage entity represents a single layer excision and examination cycle within a Mohs micrographic surgery procedure. It is identified by a stage number within its parent procedure session. The entity carries the tissue layer map, margin status for each section, and the decision to proceed with an additional stage or conclude. Each stage has its own lifecycle: excised, processed, examined, and assessed.

### Phototherapy Session

A Phototherapy Session entity represents a single light treatment within a phototherapy plan. It is identified by a session number and carries the actual dose delivered, treatment duration, body areas exposed, and any adverse reactions observed. The entity's state progresses from scheduled through administered to documented.

## Cosmetic Services Context Entities

### Cosmetic Consultation

A Cosmetic Consultation entity represents an aesthetic consultation encounter where a patient's cosmetic goals are assessed and treatment options are discussed. It is identified by a unique consultation identifier and carries the patient's aesthetic concerns, provider recommendations, and treatment plan proposals. The entity progresses from initial to plan-proposed to plan-accepted.

### Cosmetic Consent

A Cosmetic Consent entity represents the informed consent documentation for a cosmetic procedure. It is identified by a unique consent identifier and carries the procedure description, risks discussed, alternatives presented, patient acknowledgments, and signature date. The entity has a lifecycle: drafted, presented, signed, and active. A consent entity may expire or be revoked.

## Pathology Integration Context Entities

### Specimen Case

A Specimen Case entity tracks a biopsy specimen through the pathology laboratory workflow. It is identified by a specimen accession number and carries the specimen type, collection date, clinical history provided, and processing status. The entity transitions through states: collected, accessioned, processed, examined, reported, and archived.

### Chain of Custody Entry

A Chain of Custody Entry entity documents a single handling transition of a specimen. It carries the handler identity, timestamp, action performed (collected, transported, received, processed), and condition assessment. Each entry is identified by a unique entry identifier within its parent specimen case.

## Treatment Outcomes Context Entities

### Treatment Outcome Record

A Treatment Outcome Record entity links a specific treatment to its measured outcomes over time. It is identified by a unique outcome identifier and carries the originating treatment reference, treatment modality, start date, and current assessment status. The entity accumulates severity assessments and wound healing observations over its lifecycle.

## Entity Identity Strategies

Following Vernon's (2013) guidance, entity identifiers in the dermatology domain are generated by the system rather than derived from natural attributes. Lesion locations and patient demographics are attributes, not identifiers, because they can change or be shared. Unique identifiers ensure unambiguous reference across bounded contexts and through the complete lifecycle of each entity.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 5 on entity design and identity.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on modeling entities within aggregates.
