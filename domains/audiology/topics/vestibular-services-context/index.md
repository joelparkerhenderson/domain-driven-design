# Vestibular Services Context

## Overview

The Vestibular Services Context handles balance assessment, vestibular diagnostics, and vestibular rehabilitation for patients presenting with dizziness, vertigo, imbalance, or fall risk. This bounded context addresses the vestibular component of audiologic practice, recognizing that the auditory and vestibular systems share anatomical structures within the inner ear. Many patients with hearing loss also present with vestibular dysfunction, and audiologists are uniquely positioned to evaluate both systems.

## Strategic Importance

As a supporting subdomain, the Vestibular Services Context serves a specialized but clinically important patient population. Vestibular disorders significantly impact quality of life and pose safety risks through falls. The domain model must capture the complexity of vestibular assessment batteries, diagnostic reasoning for localizing lesion sites, and evidence-based rehabilitation protocols.

## Ubiquitous Language

Key terms within this bounded context include: vestibular system, semicircular canal, otolith organ, utricle, saccule, nystagmus, vertigo, dizziness, imbalance, videonystagmography (VNG), electronystagmography (ENG), caloric testing, positional testing, Dix-Hallpike maneuver, benign paroxysmal positional vertigo (BPPV), vestibular evoked myogenic potentials (VEMP), rotary chair testing, unilateral weakness, directional preponderance, vestibular rehabilitation therapy (VRT), habituation exercises, gaze stabilization exercises, balance retraining, fall risk, and canalith repositioning procedure (CRP).

## Aggregates

### Vestibular Evaluation

The Vestibular Evaluation is the central aggregate, rooted by the VestibularEvaluation entity. It contains VNGResult value objects (oculomotor testing, positional testing, caloric testing), CaloricResponse value objects (slow phase velocities for each ear and temperature condition), PositionalTestResult value objects (Dix-Hallpike, roll test), and VestibularDiagnosis value objects. The aggregate enforces that all required components of the test battery are completed before a comprehensive diagnosis is assigned.

### Vestibular Rehabilitation Program

The Vestibular Rehabilitation Program aggregate tracks a VRT program. Rooted by the VestibularRehabilitationProgram entity, it contains ExercisePrescription value objects (habituation exercises, gaze stabilization exercises, balance retraining activities), ComplianceRecord value objects, and FunctionalImprovement value objects (Dynamic Gait Index scores, Dizziness Handicap Inventory scores). The aggregate enforces that exercise prescriptions are appropriate for the diagnosed vestibular condition.

## Value Objects

NystagmusRecording captures eye movement parameters (direction, velocity, amplitude, frequency) during vestibular testing. CaloricResponse records the vestibular response to thermal stimulation and enables Jongkees formula calculation. PositionalTestResult records the presence, direction, and characteristics of position-evoked nystagmus. VestibularDiagnosis encapsulates the diagnostic conclusion (peripheral vs. central, unilateral vs. bilateral, site of lesion). FallRiskScore quantifies fall risk based on clinical assessment. DizzinessHandicapInventoryScore captures patient-reported dizziness impact.

## Domain Events

VestibularEvaluationCompleted is published when a vestibular assessment is finalized, triggering documentation and potential rehabilitation referral. FallRiskIdentified is published when assessment identifies elevated fall risk, triggering safety counseling and appropriate referrals. BPPVDiagnosed is published when benign paroxysmal positional vertigo is identified, triggering canalith repositioning treatment. VRTProgressRecorded is published when rehabilitation milestones are achieved.

## Domain Services

The UnilateralWeaknessCalculationService applies the Jongkees formula to caloric test responses to quantify asymmetry in vestibular function between the two ears. The FallRiskAssessmentService combines vestibular findings, patient demographics, and functional assessment data to produce a comprehensive fall risk evaluation. The ExercisePrescriptionService generates appropriate VRT exercises based on the vestibular diagnosis, patient capabilities, and rehabilitation goals.

## Business Rules

Caloric testing requires temperature-controlled water or air irrigation at specified temperatures (30 degrees C and 44 degrees C for water, or equivalent for air). Unilateral weakness exceeding 20-25% (depending on laboratory norms) is considered clinically significant. Positional testing must distinguish between BPPV (geotropic or apogeotropic nystagmus with latency and fatigue) and central positional nystagmus (persistent, non-fatiguing, direction-changing). Vestibular rehabilitation exercises must be progressed systematically based on patient tolerance and functional improvement. Patients with acute vestibular symptoms must be screened for central causes requiring urgent medical referral.

## Diagnostic Test Battery

The domain model supports a comprehensive vestibular test battery. Oculomotor testing evaluates saccades, smooth pursuit, optokinetic nystagmus, and gaze-evoked nystagmus to identify central vestibular pathology. Positional testing (Dix-Hallpike, supine roll test) identifies BPPV variants. Caloric testing stimulates each horizontal semicircular canal independently to assess lateral canal function. VEMPs assess otolith organ function (cervical VEMPs for saccular function, ocular VEMPs for utricular function). Rotary chair testing provides a comprehensive assessment of the vestibulo-ocular reflex across frequencies.

## Integration with Other Contexts

The Vestibular Services Context operates relatively independently from the hearing-focused contexts but shares important connections. Patients often present with both hearing and balance complaints, and the shared anatomy of the inner ear means that some conditions (such as Meniere's disease or labyrinthitis) affect both systems. When vestibular evaluation reveals hearing implications, events are published that the Hearing Assessment Context can consume. The Vestibular Services Context conforms to the Rehabilitation Context's model for exercise tracking and outcome measurement when vestibular rehabilitation programs overlap with general aural rehabilitation services.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Jacobson, G. P., & Shepard, N. T. (Eds.) (2016). Balance Function Assessment and Management. 2nd Edition. Plural Publishing.
- Herdman, S. J., & Clendaniel, R. A. (2014). Vestibular Rehabilitation. 4th Edition. F.A. Davis.
