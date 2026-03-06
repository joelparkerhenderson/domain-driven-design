# Lesion Management Context

The Lesion Management Context is a core bounded context in the dermatology domain that tracks individual skin lesions through their complete lifecycle from initial identification through diagnosis, treatment planning, and long-term follow-up. It owns biopsy decisions, excision planning with margin requirements, follow-up scheduling based on risk stratification, and the longitudinal history of each managed lesion. This context coordinates with both the Clinical Assessment Context (upstream) and the Procedure Management and Pathology Integration Contexts (downstream).

## Context Purpose

The Lesion Management Context exists to manage the clinical decision-making and coordination that surrounds individual lesions requiring attention beyond routine observation. When a skin examination identifies a lesion of concern, the Lesion Management Context takes ownership of that lesion's management, tracking every decision, biopsy, diagnosis, and follow-up through to resolution or ongoing monitoring. This context encodes the clinical protocols and guidelines that govern how lesions are evaluated and treated.

## Aggregates

### Lesion Record Aggregate

The primary aggregate is the Lesion Record. The root entity is identified by a unique lesion identifier and maintains the lesion's current state, confirmed diagnosis, assessment history, biopsy records, treatment references, and follow-up schedule. The aggregate contains Biopsy Decision entities that capture each decision to biopsy the lesion (including indication, selected technique, and outcome), and Excision Plan entities that define surgical treatment parameters. The Lesion Record enforces critical invariants: an excision plan cannot be finalized without a confirmed diagnosis; biopsy decisions must include a documented clinical indication; and follow-up intervals must comply with guideline-based risk stratification.

### Follow-Up Schedule Aggregate

The Follow-Up Schedule aggregate manages the temporal coordination of monitoring visits for all active lesions belonging to a patient. It enforces scheduling rules that prevent conflicting appointments, ensure high-risk lesions receive appropriately frequent follow-up, and consolidate monitoring visits when multiple lesions can be assessed in a single encounter. The aggregate works with Follow-Up Interval value objects that carry the recommended interval duration, risk category, and guideline source.

## Key Entities

### Lesion Record Entity

The Lesion Record entity transitions through a defined lifecycle: identified (newly documented from clinical assessment), under-observation (being monitored without intervention), biopsy-pending (biopsy decision made, awaiting procedure), biopsy-completed (specimen collected, awaiting pathology results), diagnosed (definitive diagnosis confirmed from pathology or clinical assessment), treatment-planned (excision or other treatment planned), treated (treatment performed), in-follow-up (post-treatment monitoring active), resolved (lesion successfully treated with no further action needed), or recurred (previously treated lesion has returned).

Each state transition represents a significant clinical event and triggers appropriate domain events. The entity enforces transition rules: for example, a lesion cannot move from identified to treatment-planned without passing through diagnosed (unless the clinical assessment alone provides sufficient diagnostic certainty for certain benign conditions).

### Biopsy Decision Entity

The Biopsy Decision entity captures the clinical reasoning behind the decision to sample a lesion. It carries the indication category (diagnostic uncertainty, rule out malignancy, monitor treatment response, confirm clinical diagnosis), the selected biopsy type (shave, punch with size, incisional, or excisional), target depth, and the decision rationale. The entity progresses through states: proposed, reviewed, approved, scheduled, performed, and completed.

### Excision Plan Entity

The Excision Plan entity defines the surgical removal parameters for a lesion. It carries the planned Surgical Margin value object (with guideline-recommended and planned widths), the excision technique (standard elliptical, Mohs micrographic, wide local excision), the reconstruction approach (primary closure, flap, graft, secondary intention), and any special considerations (proximity to vital structures, cosmetic concerns). The entity evolves from draft through review, approval, and execution.

## Key Value Objects

The Surgical Margin value object carries the recommended margin width in millimeters, the guideline source (such as NCCN or BAD guidelines), the diagnosis-specific rationale, and the actual achieved margin post-excision. The Follow-Up Interval carries the recommended duration between visits, the risk category that determined the interval, and the guideline source. The Biopsy Type Classification carries the technique name, applicable instrument size, expected depth, and typical clinical scenarios.

## Domain Events Published

BiopsyDecisionMade is published when a clinical decision to biopsy is documented. BiopsyPerformed is published when a specimen is collected. LesionDiagnosisConfirmed is published when a definitive diagnosis is established. ExcisionPlanFinalized is published when surgical parameters are approved. RecurrenceDetected is published when a previously treated lesion recurs.

## Domain Events Consumed

LesionIdentified from the Clinical Assessment Context triggers creation of a new Lesion Record. SuspiciousLesionFlagged from Clinical Assessment prioritizes biopsy evaluation. PathologyReportCompleted from Pathology Integration updates the lesion's diagnosis and staging.

## Domain Services

The BiopsyIndicationService evaluates whether a lesion meets biopsy criteria based on its characteristics, history, and guidelines. The MarginCalculationService determines recommended surgical margins based on diagnosis, staging, and location. The FollowUpIntervalDeterminationService calculates monitoring frequency based on risk stratification.

## Clinical Guidelines Integration

This context integrates established clinical guidelines for lesion management decisions. Guidelines from the National Comprehensive Cancer Network (NCCN) for melanoma and non-melanoma skin cancer, the American Academy of Dermatology (AAD) for various conditions, and the British Association of Dermatologists (BAD) inform margin requirements, biopsy indications, and follow-up intervals. These guidelines are represented as configurable rule sets within the domain services rather than hardcoded into entity logic.

## Relationships to Other Contexts

The Clinical Assessment Context is upstream, providing lesion findings. The Procedure Management Context is downstream, receiving procedure requests from finalized excision plans. The Pathology Integration Context receives biopsy specimens and returns diagnostic results. The Treatment Outcomes Context initiates outcome tracking when a lesion diagnosis is confirmed and treatment begins.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- National Comprehensive Cancer Network. *NCCN Guidelines: Melanoma, Basal Cell Skin Cancer, Squamous Cell Skin Cancer*.
- Swetter, Susan M., et al. "Guidelines of Care for the Management of Primary Cutaneous Melanoma." Journal of the American Academy of Dermatology, 2019.
