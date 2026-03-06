# Clinical Assessment Context

The Clinical Assessment Context is a core bounded context in the dermatology domain that encompasses the initial and ongoing evaluation of a patient's skin health. It owns the patient skin profile, full-body and focused skin examinations, dermoscopic evaluations, lesion documentation with anatomical mapping, and classification using ICD-10 dermatology codes and morphological descriptors. This context serves as the entry point for most clinical workflows in dermatology practice.

## Context Purpose

The Clinical Assessment Context exists to capture the clinical reasoning and observational data that dermatologists produce during skin evaluations. Every clinical decision in dermatology begins with assessment: examining the skin, identifying abnormalities, characterizing their appearance, and determining which findings warrant further investigation or monitoring. This context models that diagnostic process using the ubiquitous language of clinical dermatology.

## Aggregates

### Skin Examination Aggregate

The primary aggregate is the Skin Examination. The root entity carries the examination identifier, patient reference, encounter date, examining provider, and examination type (full-body, focused, or follow-up). Within this aggregate, Lesion Finding entities represent individual abnormalities identified during the examination. Each Lesion Finding carries an Anatomical Location value object, Lesion Measurement value object, Morphological Descriptor value object, and optionally a Dermoscopy Evaluation value object. The aggregate enforces the invariant that every lesion finding must have a valid anatomical location and at least one morphological characteristic documented.

### Patient Skin Profile Aggregate

The Patient Skin Profile aggregate maintains the longitudinal record of a patient's skin health baseline. It carries the Fitzpatrick Skin Type value object, documented skin conditions (chronic conditions such as psoriasis, eczema, rosacea), relevant medication history (photosensitizing drugs, immunosuppressants), family history of skin cancer, and a history of sun exposure and tanning behavior. This aggregate enforces the invariant that a Fitzpatrick skin type must be established during the initial clinical encounter.

## Key Entities

The Skin Examination entity transitions through states: scheduled, in-progress, documented, reviewed, and finalized. A finalized examination cannot be modified; amendments require a new examination record with a reference to the original. The Lesion Finding entity captures a point-in-time observation of a specific lesion. When the same lesion is observed across multiple examinations, each observation is a separate Lesion Finding linked through a common lesion reference identifier to enable longitudinal tracking.

## Key Value Objects

The Anatomical Location value object uses a hierarchical body region system: major region (head, neck, trunk, upper extremity, lower extremity), subregion (specific area such as left anterior forearm), and laterality. The Morphological Descriptor captures shape, border, color, surface, and elevation. The ABCDE Score provides structured melanoma screening assessment. The Dermoscopic Pattern captures pattern type, associated structures, and colors observed under dermoscopy.

## Domain Events Published

The Clinical Assessment Context publishes several domain events. LesionIdentified is published when a new lesion finding is documented. SuspiciousLesionFlagged is published when a finding meets heightened concern criteria. SkinExaminationCompleted is published when an examination is finalized. These events notify downstream contexts, particularly the Lesion Management Context, of findings that require further action.

## Domain Services

The LesionRiskAssessmentService evaluates the risk level of a lesion finding by correlating its morphological descriptors, ABCDE score, dermoscopic pattern, and the patient's skin type and history. The DermoscopyPatternAnalysisService applies dermoscopic algorithms to suggest differential diagnoses based on observed pattern combinations.

## Classification Systems

The Clinical Assessment Context uses ICD-10 Chapter XII codes (L00-L99) for diseases of the skin and subcutaneous tissue, along with codes from other chapters for neoplasms (C43-C44 for melanoma and non-melanoma skin cancer). SNOMED CT concepts provide additional clinical specificity. The context maintains a classification service that validates code applicability and suggests appropriate codes based on documented findings.

## Relationships to Other Contexts

This context is upstream to the Lesion Management Context, which consumes lesion findings for tracking and management. It has a shared kernel with the Treatment Outcomes Context for clinical scoring systems. The Cosmetic Services Context conforms to this context's patient skin profile model for baseline patient information. Treatment outcome data from previous encounters feeds back into this context through a published language to inform current assessments.

## Clinical Workflow

A typical workflow begins with the patient presenting for either a scheduled skin check or a specific complaint. The dermatologist creates a Skin Examination, performs a systematic evaluation (full-body or focused), documents findings as Lesion Finding entities with anatomical locations and morphological characteristics, performs dermoscopy on concerning lesions, assesses risk using the ABCDE criteria and dermoscopic pattern analysis, and finalizes the examination with an overall impression and recommended actions. Any suspicious findings trigger domain events that initiate workflows in the Lesion Management Context.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Bolognia, Jean L., et al. *Dermatology*. Elsevier, 4th Edition, 2017.
- Kittler, Harald, et al. "Dermoscopy: Introduction and Review." Journal of the American Academy of Dermatology, 2016.
