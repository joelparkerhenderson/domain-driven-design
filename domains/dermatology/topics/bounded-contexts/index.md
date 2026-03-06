# Bounded Contexts in the Dermatology Domain

Bounded contexts are logical boundaries within which a specific domain model and its terminology remain consistent. In the dermatology domain, six bounded contexts reflect the natural divisions of clinical practice, each owning its own model, rules, and lifecycle. As Evans (2003) describes, a bounded context ensures that terms like "lesion" or "assessment" carry precise, unambiguous meaning within their context, even when the same term appears in other contexts with different semantics.

## The Six Bounded Contexts

### Clinical Assessment Context

This context encompasses the initial and ongoing patient encounter for skin evaluation. It owns the patient skin profile, full-body skin examination records, dermoscopic evaluations, lesion documentation with anatomical body mapping, and classification using ICD-10 dermatology codes and morphological descriptors. The primary aggregate is the Skin Examination, which contains lesion findings as entities. Dermoscopic patterns, ABCDE criteria scores, and Fitzpatrick skin type are modeled as value objects. This context serves as the entry point for most clinical workflows.

### Lesion Management Context

This context tracks individual lesions through their full lifecycle from initial identification through diagnosis, treatment planning, and follow-up. It owns biopsy decisions, excision planning with margin requirements, and follow-up scheduling based on risk stratification. The primary aggregate is the Lesion Record, which maintains the history of assessments, biopsies, and treatments for a single lesion. The concept of "lesion" in this context is richer than in the Clinical Assessment Context, carrying treatment history and prognostic information.

### Procedure Management Context

This context manages the execution of specialized dermatologic procedures. It owns Mohs micrographic surgery stages and layer tracking, cryotherapy session parameters, laser therapy protocols and equipment settings, phototherapy (UVB/PUVA) treatment plans, and electrosurgery records. The primary aggregate is the Procedure Session, which encapsulates all parameters, steps, and immediate outcomes of a single procedural encounter. Each procedure type has specific value objects for its unique parameters.

### Cosmetic Services Context

This context handles elective aesthetic services, which operate under different regulatory, consent, and billing frameworks than medical dermatology. It owns aesthetic consultation records, injectable treatment plans (neuromodulators and dermal fillers), chemical peel protocols, microneedling sessions, and skin rejuvenation programs. The primary aggregate is the Cosmetic Treatment Plan, which tracks the patient's aesthetic goals, treatment areas, products used (including lot numbers), and outcomes. Cosmetic consent is a distinct entity with its own lifecycle.

### Pathology Integration Context

This context coordinates the flow of information between dermatology clinical practice and pathology laboratory services. It owns specimen tracking from collection through accessioning, histopathology report integration, chain of custody documentation, and TNM staging for cutaneous malignancies. The primary aggregate is the Specimen Case, which tracks a biopsy specimen from the moment of collection through final diagnostic reporting. This context typically integrates with external laboratory information systems through anti-corruption layers.

### Treatment Outcomes Context

This context measures and tracks the effectiveness of dermatologic treatments over time. It owns standardized scoring systems (PASI for psoriasis, SCORAD for atopic dermatitis, DLQI for quality of life), wound healing progression tracking, recurrence monitoring with risk stratification, and clinical photo comparison over time. The primary aggregate is the Treatment Outcome Record, which links a specific treatment to longitudinal effectiveness measurements. This context supports evidence-based practice by enabling comparison across treatment modalities.

## Context Ownership and Team Alignment

Each bounded context should be owned by a team or team segment that has deep knowledge of that area. Vernon (2013) emphasizes that bounded contexts often align with team boundaries. In a dermatology practice, the clinical assessment team (dermatologists, residents, physician assistants), surgical team (Mohs surgeons, surgical staff), cosmetic team (aesthetic practitioners), and pathology coordination team each bring specialized expertise to their respective contexts.

## Shared Concepts Across Contexts

Certain concepts appear in multiple bounded contexts but carry different meanings. The concept of "patient" is a shared identifier but carries different attributes in each context. In Clinical Assessment, the patient is associated with skin type and examination history. In Cosmetic Services, the patient is associated with aesthetic goals and consent records. In Treatment Outcomes, the patient is associated with longitudinal scoring data. Each context maintains its own patient representation through an anti-corruption layer or shared kernel for the minimal common identifier.

## Context Boundaries and Autonomy

Each bounded context operates autonomously, managing its own data and enforcing its own business rules. Cross-context communication occurs through domain events rather than direct model sharing. For example, when a biopsy is performed (Lesion Management Context), a BiopsyPerformed event notifies the Pathology Integration Context to initiate specimen tracking, without requiring the Lesion Management Context to understand pathology laboratory workflows.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded contexts and their relationship to teams.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on bounded context boundaries.
- American Academy of Dermatology. *Practice Management Resources*.
