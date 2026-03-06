# Treatment Outcomes Context

The Treatment Outcomes Context is a supporting bounded context in the dermatology domain that measures and tracks the effectiveness of dermatologic treatments over time. It owns standardized severity scoring systems (PASI, SCORAD, DLQI), wound healing progression tracking, recurrence monitoring with risk stratification, and clinical photo comparison across time points. This context supports evidence-based practice by enabling longitudinal analysis across treatment modalities and contributing to protocol refinement.

## Context Purpose

The Treatment Outcomes Context exists because dermatology treatment effectiveness must be measured systematically and longitudinally. A psoriasis treatment's success is determined by PASI score reduction over months, not by a single observation. A skin cancer excision's success depends on margin status and recurrence monitoring over years. Wound healing after Mohs surgery progresses through defined phases that must be tracked. This context captures the temporal dimension of dermatology care, converting point-in-time clinical observations into meaningful outcome assessments.

## Aggregates

### Treatment Outcome Record Aggregate

The primary aggregate is the Treatment Outcome Record. The root entity carries the outcome identifier, patient reference, treatment reference (linking to the originating procedure or treatment plan in other contexts), treatment modality (surgery, cryotherapy, laser, phototherapy, injectable, topical, systemic), diagnosis being treated, and the baseline severity assessment at treatment initiation. Within this aggregate, SeverityAssessment entities capture standardized scores at defined intervals. WoundHealingAssessment entities track post-procedural tissue repair. RecurrenceAssessment entities document monitoring evaluations for treated malignancies.

The aggregate enforces invariants that ensure measurement consistency. All severity assessments within a single outcome record must use the same scoring system. Assessment intervals must follow the monitoring protocol defined for the treatment modality and diagnosis. Baseline assessments must be recorded before or at the time of treatment initiation. Recurrence assessments must include documentation of the examination method and findings.

### Photo Comparison Series Aggregate

The Photo Comparison Series aggregate manages longitudinal clinical photography for outcome documentation. The root entity carries the series identifier, patient reference, treatment reference, and anatomical region being documented. PhotoTimepoint entities within the aggregate record individual photographs with standardized capture parameters (lighting, distance, angle, camera settings), capture date, and associated clinical notes. The aggregate enforces standardization rules: photographs within a series must maintain consistent capture parameters to enable valid comparison.

## Key Entities

### Treatment Outcome Record Entity

The Treatment Outcome Record entity has a lifecycle that spans the duration of treatment and follow-up. It begins in the baseline state when the initial severity assessment is recorded. It progresses to active-monitoring during treatment as sequential assessments are recorded. It may reach concluded when the treatment course ends and a final assessment determines the overall response. For malignancy treatments, the entity transitions to surveillance when active treatment ends and long-term recurrence monitoring begins. The entity is closed when the monitoring period ends without recurrence, or reopened if recurrence is detected.

### Severity Assessment Entity

The Severity Assessment entity captures a single standardized scoring event. For psoriasis, it records the full PASI calculation: area and intensity scores for head, trunk, upper extremities, and lower extremities, producing a composite score from 0 to 72. For atopic dermatitis, it records the SCORAD index: extent (0-100 percent), intensity (0-18), and subjective symptoms (0-20). For quality of life measurement, it records the DLQI: ten question responses producing a total from 0 to 30. Each assessment carries the assessment date, assessing provider, and any relevant notes about the patient's condition at the time of assessment.

### Wound Healing Assessment Entity

The Wound Healing Assessment entity tracks tissue repair following dermatologic procedures. It carries the assessment date, wound dimensions (length, width, depth), healing phase classification (inflammatory, proliferative, remodeling), tissue characteristics (granulation, epithelialization, contraction), presence of complications (infection, dehiscence, hypertrophic scarring), and wound care compliance assessment. The entity progression reflects normal healing timelines for the specific procedure type and anatomical location.

### Recurrence Assessment Entity

The Recurrence Assessment entity documents each monitoring evaluation for previously treated malignancies. It carries the assessment date, examination method (clinical, dermoscopic, imaging), examination findings, recurrence status (no evidence of recurrence, suspected recurrence, confirmed recurrence), and the time interval since treatment. If recurrence is detected, the entity captures recurrence characteristics and triggers a RecurrenceDetected domain event.

## Key Value Objects

The PASI Score value object carries component scores for four body regions and the composite total, with severity banding (clear: 0, mild: less than 5, moderate: 5-10, severe: greater than 10). The SCORAD Index value object carries extent, intensity, and symptom components with the composite total. The DLQI Score value object carries individual question responses and the total with impact banding. The Treatment Response Classification value object categorizes the response as complete, partial, minimal, none, or worsened based on percentage change from baseline.

## Domain Events Published

SeverityAssessmentRecorded is published when a standardized score is documented, carrying the scoring system, score value, and change from previous assessment. TreatmentResponseClassified is published when an outcome is formally categorized. RecurrenceDetected is published when surveillance identifies a recurrence. WoundHealingMilestoneReached is published when a wound transitions between healing phases.

## Domain Events Consumed

ProcedureCompleted from the Procedure Management Context triggers creation of a Treatment Outcome Record and initiates wound healing tracking. LesionDiagnosisConfirmed from the Lesion Management Context provides the diagnosis context for outcome tracking. InjectableTreatmentAdministered from the Cosmetic Services Context may initiate cosmetic outcome tracking. SkinExaminationCompleted from the Clinical Assessment Context provides follow-up assessment data that feeds into severity scoring.

## Domain Services

The TreatmentResponseEvaluationService compares sequential severity scores to classify treatment response. The OutcomeComparisonService analyzes outcomes across different treatment modalities for similar diagnoses to support evidence-based protocol selection.

## Longitudinal Analysis

This context supports longitudinal analysis that spans months or years. For chronic conditions like psoriasis, treatment outcomes are tracked across multiple treatment courses. For malignancies, surveillance continues for years post-treatment. The domain model supports this by maintaining the complete assessment history within the Treatment Outcome Record aggregate and enabling comparison across time points, treatment modalities, and patient populations.

## Relationships to Other Contexts

The Procedure Management Context and Cosmetic Services Context are upstream, providing treatment completion data. The Clinical Assessment Context has a shared kernel for severity scoring systems and provides follow-up assessment data through a published language. The Lesion Management Context provides diagnosis context and receives recurrence notifications. The Pathology Integration Context provides staging information that informs outcome tracking protocols.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Fredriksson, T., and Pettersson, U. "Severe Psoriasis: Oral Therapy with a New Retinoid." Dermatologica, 1978. (Original PASI publication.)
- Finlay, A.Y., and Khan, G.K. "Dermatology Life Quality Index (DLQI): A Simple Practical Measure for Routine Clinical Use." Clinical and Experimental Dermatology, 1994.
