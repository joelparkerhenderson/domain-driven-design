# Radiation Therapy Context

## Overview

The Radiation Therapy Context manages the planning and delivery of therapeutic radiation for cancer treatment. This bounded context translates treatment plan directives into detailed dosimetric plans, manages the simulation and setup process, schedules and tracks fractionated treatment delivery, and supports advanced techniques including image-guided radiation therapy (IGRT) and brachytherapy. Radiation therapy requires extreme precision, and the domain model must capture the clinical constraints that ensure safe and effective dose delivery.

## Ubiquitous Language

Within this context, a "plan" refers specifically to the radiation dosimetric plan, not the broader treatment plan from the Treatment Planning Context. A "fraction" is a single treatment delivery session. "Simulation" is the planning session where the patient is positioned and imaged to define treatment geometry. "Target volume" is the anatomic region to be treated, defined in layers: GTV (gross tumor volume), CTV (clinical target volume), and PTV (planning target volume). "Organs at risk" (OAR) are healthy structures near the target that must be protected from excessive radiation dose.

## Aggregate Roots

### Radiation Treatment Plan

The Radiation Treatment Plan is the primary aggregate root. It contains the target volume definitions (GTV, CTV, PTV) with their geometric parameters, the organs at risk with specified dose constraints, the prescribed total dose and dose per fraction, the fractionation schedule, the delivery technique (3D conformal, IMRT, VMAT, stereotactic), and the computed dose distribution.

The aggregate enforces critical invariants. The dose to each organ at risk must not exceed its specified constraint unless explicitly documented with clinical justification. The fractionation schedule must deliver the prescribed total dose (number of fractions multiplied by dose per fraction equals total dose). The PTV must fully encompass the CTV with the specified margin. The plan must be approved by a radiation oncologist and verified by a medical physicist before treatment can begin.

### Brachytherapy Plan

The Brachytherapy Plan aggregate manages internal radiation therapy where sealed sources are placed inside or adjacent to the tumor. It contains the source type and activity, the applicator geometry, the dwell positions and times, the computed dose distribution around the sources, and the prescribed dose to the reference point. This aggregate is separate from the external beam plan because brachytherapy has fundamentally different planning constraints and delivery workflows.

## Key Entities

**Treatment Fraction**: An entity representing a single radiation treatment delivery session. Each fraction is identified within the treatment course and records the planned dose, the delivered dose, imaging verification results (for IGRT), patient positioning accuracy, and any treatment interruptions or deviations. Fractions progress through states: scheduled, patient-setup, imaged, treatment-delivered, verified.

**Simulation Session**: An entity representing the planning session where the patient is positioned and imaged. Records the immobilization devices used (thermoplastic mask, vacuum cushion), the imaging modality (CT simulation, MRI simulation), the acquired image dataset reference, and the treatment isocenter coordinates.

**Treatment Course**: An entity representing the complete series of fractions prescribed for a patient. Tracks the overall progress (fractions delivered out of total planned), treatment breaks, and cumulative delivered dose.

## Key Value Objects

**Radiation Dose**: A dose quantity in Gray (Gy) or centigray (cGy) with a type designation (prescribed, planned, delivered).

**Target Volume Definition**: The geometric definition of a treatment target, including the volume type (GTV, CTV, PTV), the delineation method, and the expansion margins applied.

**Dose-Volume Constraint**: A constraint on radiation dose to a specific structure, expressed as a volume percentage receiving a specified dose threshold (for example, lung V20 less than 30 percent).

**Fractionation Scheme**: The prescribed fractionation pattern including total dose, dose per fraction, number of fractions, and frequency.

**Isocenter Coordinates**: The three-dimensional reference point used for treatment setup, defined relative to the patient's anatomy.

## Domain Events

**SimulationCompleted**: The radiation therapy simulation session has been completed.
**TargetVolumesDelineated**: The radiation oncologist has defined the target volumes on the simulation images.
**RadiationPlanCreated**: A new dosimetric plan has been computed by the treatment planning system.
**RadiationPlanApproved**: The dosimetric plan has passed clinical review by the radiation oncologist and physics verification.
**FractionDelivered**: A single treatment fraction has been administered and verified.
**TreatmentBreakRecorded**: A planned treatment fraction was not delivered due to a machine, patient, or clinical reason.
**RadiationCourseCompleted**: All planned fractions have been delivered.
**AdaptiveReplanTriggered**: Changes in patient anatomy or tumor response necessitate creating a new treatment plan mid-course.

## Domain Services

**DoseConstraintEvaluationService**: Evaluates a radiation treatment plan against defined dose-volume constraints for organs at risk. Returns a compliance assessment for each constraint.

**FractionationRecommendationService**: Recommends an appropriate fractionation scheme based on the treatment site, intent, and clinical factors.

## Integration Points

This context is downstream of the Treatment Planning Context, receiving directives for radiation therapy as a treatment modality. The translation between contexts maps the treatment plan's radiation therapy selection (site, intent, timing) into the radiation context's simulation and planning workflow.

This context interfaces with imaging systems through an anti-corruption layer that handles DICOM data for both simulation imaging and treatment verification imaging. It interfaces with treatment delivery systems (linear accelerators) for dose delivery tracking. The Radiation Therapy Context publishes treatment completion events that the Survivorship Care Context consumes for treatment summary generation.

## Technical Precision

Radiation therapy demands sub-millimeter precision in treatment delivery. The domain model reflects this precision through value objects that carry appropriate units and precision. Dose values carry units (Gy or cGy) and meaningful decimal precision. Position values carry units (millimeters) and reflect the precision achievable with the imaging and delivery equipment. The model does not perform dose computation itself (that is done by specialized treatment planning systems), but it validates and tracks the results of those computations.

## Quality Assurance

The domain model supports the quality assurance workflow inherent in radiation therapy. Plan verification by a medical physicist is modeled as a required state transition in the Radiation Treatment Plan aggregate. Treatment fraction delivery includes imaging verification as a required step in the IGRT workflow. These QA requirements are domain invariants, not optional process steps.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- International Commission on Radiation Units and Measurements. ICRU Report 83: Prescribing, Recording, and Reporting Photon-Beam IMRT. 2010.
- American Association of Physicists in Medicine. AAPM Task Group Reports. https://www.aapm.org/pubs/reports/
