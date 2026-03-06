# Incontinence Management Context

## Overview

The Incontinence Management Context covers the evaluation and treatment of urinary incontinence across all clinical types. This bounded context manages the diagnostic classification of incontinence, conservative management strategies, pharmacotherapy, and surgical interventions. It owns the treatment escalation model that guides patients from behavioral therapies through increasingly invasive interventions based on treatment response.

## Scope

This context covers incontinence classification (stress urinary incontinence, urge incontinence, mixed incontinence, overflow incontinence, functional incontinence, neurogenic bladder), diagnostic assessment (bladder diary, pad weight testing, urodynamic evaluation, cystoscopy), conservative management (pelvic floor muscle training, bladder training, behavioral modification), pharmacotherapy (antimuscarinics, beta-3 agonists, combination therapy), neuromodulation (sacral neuromodulation, percutaneous tibial nerve stimulation), and surgical intervention (mid-urethral slings, Burch colposuspension, artificial urinary sphincter, bladder augmentation).

## Ubiquitous Language

"Stress incontinence" is leakage with increased abdominal pressure (cough, sneeze, exertion). "Urge incontinence" is leakage associated with a sudden, compelling desire to void. "Mixed incontinence" has both stress and urge components. "Detrusor overactivity" is an urodynamic finding of involuntary bladder contractions during filling. "Pad-free" indicates complete dryness, the gold standard outcome measure. "Treatment escalation" is the progression from conservative to more invasive interventions when prior treatments fail to achieve adequate improvement.

## Aggregates

The ContinenceAssessment aggregate root captures the complete evaluation of a patient's incontinence. It contains IncontinenceClassification value objects, BladderDiary entities, PadWeightTest value objects, PelvicFloorAssessment entities, and UrodynamicReferral entities. The TreatmentPlan aggregate manages the selected treatment approach with escalation criteria. The SurgicalOutcome aggregate tracks post-operative continence results.

## Key Entities

BladderDiaryEntry entities record daily voiding patterns, leakage episodes, and fluid intake. TherapySession entities track individual pelvic floor rehabilitation appointments. MedicationTrial entities record pharmacotherapy attempts with dosing, duration, response, and side effects. DeviceImplant entities track neuromodulation devices with programming parameters and battery status.

## Value Objects

PadWeightResult captures leakage severity by standardized pad testing. ValsalvaLeakPointPressure measures the abdominal pressure threshold for stress leakage. MaximumCystometricCapacity records functional bladder capacity from urodynamic testing. IncontinenceSeverityIndex combines frequency and amount of leakage into a validated severity score. QualityOfLifeScore captures the impact of incontinence on daily activities using validated instruments (IIQ-7, UDI-6, ICIQ).

## Domain Events

ContinenceStatusChanged records transitions in continence status (improvement or worsening). PelvicFloorTherapyMilestoneReached indicates progress in rehabilitation. MedicationTrialCompleted records the outcome of a pharmacotherapy attempt. SurgicalInterventionRequested sends a referral to the Surgical Management Context when conservative measures fail. NeuromodulationProgrammingAdjusted records changes to implanted device parameters.

## Treatment Escalation Model

The context implements a structured treatment escalation pathway. First-line treatment includes behavioral modification, bladder training, and pelvic floor muscle exercises with or without biofeedback. Second-line treatment adds pharmacotherapy (antimuscarinics such as oxybutynin or solifenacin, or beta-3 agonists such as mirabegron). Third-line treatment includes sacral neuromodulation or percutaneous tibial nerve stimulation. Fourth-line treatment involves surgical intervention (mid-urethral sling for stress incontinence, augmentation cystoplasty for refractory detrusor overactivity). Escalation requires documented failure of the current level, defined as inadequate improvement after a minimum treatment duration.

## Pelvic Floor Rehabilitation Model

The pelvic floor rehabilitation model tracks exercise compliance, muscle strength progression (Oxford scale, perineometry measurements), biofeedback parameters, and functional outcomes. The model defines treatment milestones at regular intervals and assesses whether the patient is responding adequately to conservative management or should be considered for escalation.

## Integration Points

The Incontinence Management Context receives urodynamic study results from the Clinical Assessment Context, which provide the objective data for incontinence classification. It sends surgical referrals to the Surgical Management Context for sling procedures, artificial sphincter placement, and sacral neuromodulation implantation. It integrates with pharmacy systems for medication management and with physical therapy systems for pelvic floor rehabilitation tracking.

## Business Rules

Incontinence classification must be supported by objective evidence (urodynamic findings for urge versus stress differentiation). Surgical intervention for stress incontinence requires documented failure of conservative management for a minimum of three months. Antimuscarinics are contraindicated in patients with uncontrolled narrow-angle glaucoma or urinary retention. Sacral neuromodulation requires a successful percutaneous nerve evaluation trial before permanent implantation. Post-operative pad-free status must be assessed at standardized intervals (1, 3, 6, and 12 months).

## Relationship to Other Contexts

The Incontinence Management Context is a customer of the Clinical Assessment Context for diagnostic data and a customer of the Surgical Management Context for procedural services. It operates independently from the Oncologic and Stone Disease contexts, though incontinence may be a consequence of oncologic surgery (post-prostatectomy incontinence), requiring coordination with the Oncologic Context for patient history.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Lightner, D.J. et al. "AUA/SUFU Guideline: Diagnosis and Treatment of Overactive Bladder in Adults." Journal of Urology, 2019.
- Kobashi, K.C. et al. "AUA/SUFU Guideline: Surgical Treatment of Female Stress Urinary Incontinence." Journal of Urology, 2017.
