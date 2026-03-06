# Aggregates in the Dermatology Domain

Aggregates are clusters of related domain objects treated as a single unit for consistency and transactional purposes. Each aggregate has a root entity that serves as the sole entry point for all external interactions. In the dermatology domain, aggregates encapsulate the business rules and invariants that govern clinical assessment, lesion management, procedural execution, cosmetic treatment, pathology coordination, and outcome measurement. As Evans (2003) establishes, aggregates define consistency boundaries within which all business rules are enforced.

## Clinical Assessment Context Aggregates

### Skin Examination Aggregate

The Skin Examination is the primary aggregate in the Clinical Assessment Context. The aggregate root is the SkinExamination entity, identified by a unique examination identifier and associated with a patient identifier and encounter date. Within this aggregate, LesionFinding entities represent individual lesions discovered during the examination, each characterized by anatomical location, morphological descriptors, and dermoscopic pattern observations. The aggregate enforces the invariant that every lesion finding must have a valid anatomical location from the standardized body map. DermoscopyEvaluation value objects capture the dermoscopic assessment for each lesion finding. The aggregate ensures that the examination record is internally consistent before it is committed.

### Patient Skin Profile Aggregate

The Patient Skin Profile aggregate maintains the longitudinal skin health record for a patient. The root entity carries the patient's Fitzpatrick skin type, known skin conditions, allergy history relevant to dermatology, and a history of previous examinations. This aggregate enforces the invariant that Fitzpatrick skin type, once established, can only be modified through a formal reassessment process.

## Lesion Management Context Aggregates

### Lesion Record Aggregate

The Lesion Record is the primary aggregate in the Lesion Management Context. The root entity tracks a single lesion through its lifecycle, carrying the lesion's current diagnosis, history of assessments, biopsy records, and treatment decisions. BiopsyDecision entities within the aggregate capture the clinical reasoning and biopsy type selected. ExcisionPlan entities define margin requirements based on diagnosis. The aggregate enforces invariants such as the requirement that an excision plan cannot be finalized without a confirmed diagnosis, and that follow-up intervals must comply with risk-stratified guidelines.

### Follow-Up Schedule Aggregate

The Follow-Up Schedule aggregate manages the temporal coordination of lesion monitoring. It enforces scheduling rules based on diagnosis severity and recurrence risk, ensuring that high-risk lesions receive appropriately frequent follow-up while preventing scheduling conflicts.

## Procedure Management Context Aggregates

### Procedure Session Aggregate

The Procedure Session is the primary aggregate for all dermatologic procedures. The root entity captures the procedure type, patient identifier, performing provider, and date. Procedure-specific child entities vary by type: MohsStage entities track individual layers in Mohs surgery, CryotherapyApplication entities capture freeze-thaw cycles, LaserPass entities record equipment settings per treatment pass. The aggregate enforces procedure-specific invariants, such as the requirement that each Mohs stage must have a margin assessment before a subsequent stage can begin.

### Phototherapy Plan Aggregate

The Phototherapy Plan aggregate manages multi-session treatment protocols for UVB or PUVA therapy. The root entity defines the protocol parameters (light type, starting dose, escalation schedule), and PhototherapySession entities record each individual treatment session with actual dose delivered and any adverse reactions. The aggregate enforces dose escalation rules and maximum cumulative dose limits.

## Cosmetic Services Context Aggregates

### Cosmetic Treatment Plan Aggregate

The Cosmetic Treatment Plan aggregate manages aesthetic treatment programs. The root entity captures the patient's aesthetic goals, treatment areas, and planned interventions. InjectableTreatment entities track individual injection sessions with product type, lot number, units or volume administered, and injection sites. The aggregate enforces invariants including product expiration validation, maximum dosage limits per treatment area, and the requirement that a valid cosmetic consent must exist before any treatment can be administered.

## Pathology Integration Context Aggregates

### Specimen Case Aggregate

The Specimen Case aggregate tracks a biopsy specimen from collection through final diagnosis. The root entity carries the specimen accession number, collection details, and clinical context provided by the referring dermatologist. ChainOfCustodyEntry entities document each handling transition. The HistopathologyReport entity captures the pathologist's findings, diagnosis, and any staging information. The aggregate enforces chain of custody completeness and the invariant that a final report cannot be issued without all required staining and examination steps being completed.

## Treatment Outcomes Context Aggregates

### Treatment Outcome Record Aggregate

The Treatment Outcome Record aggregate links a specific treatment to its longitudinal effectiveness measurements. The root entity references the originating treatment and patient. SeverityAssessment entities capture standardized scores (PASI, SCORAD, DLQI) at defined intervals. WoundHealingAssessment entities track post-procedural healing progression. The aggregate enforces the invariant that severity assessments must use the same scoring system consistently within a treatment outcome record to enable valid comparison.

## Aggregate Design Principles

Vernon (2013) advises designing small aggregates that enforce true invariants. In the dermatology domain, this means resisting the temptation to create a single large "Patient" aggregate that encompasses all clinical data. Instead, each bounded context maintains its own focused aggregates with clear consistency boundaries. Cross-aggregate references use identifiers rather than direct object references, and eventual consistency through domain events handles cross-aggregate coordination.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 10 on aggregate design rules.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on aggregate design heuristics.
