# Domain Services in the Hormone Replacement Therapy Domain

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to any single entity or value object. They perform operations that span multiple aggregates or require coordination across domain concepts. In the hormone replacement therapy (HRT) domain, domain services handle the clinical decision-making and analytical operations that cross aggregate boundaries within each bounded context.

## Purpose

HRT clinical workflows frequently involve operations that reference multiple aggregates or apply domain rules that do not belong to a specific entity. Determining whether a patient's clinical profile matches a treatment protocol's eligibility criteria requires data from both the ClinicalAssessment and TreatmentProtocol aggregates. Calculating dose titration recommendations requires data from MonitoringPlan, AdverseEventRecord, and TreatmentOutcome aggregates. Domain services encapsulate these cross-aggregate operations while keeping the domain logic within the domain layer, separate from application orchestration.

## Clinical Assessment Context Services

### CandidacyEvaluationService

Evaluates whether a patient meets the criteria for HRT candidacy. This service takes a completed SymptomEvaluation, HormonePanelInterpretation, and PatientProfile, then applies clinical screening rules to produce a CandidacyStatus. The service encapsulates the complex decision logic that weighs symptom severity against risk factors, hormone levels against deficiency thresholds, and medical history against contraindication lists. It produces the final candidacy determination that no single entity owns.

### SymptomScoringService

Calculates composite symptom scores from individual instrument responses. Different instruments (Menopause Rating Scale, Kupperman Index, ADAM questionnaire for male androgen deficiency) have different scoring algorithms. This service encapsulates the scoring logic, validating responses against instrument-specific rules and producing SymptomScore value objects. The service is stateless, taking raw responses and returning calculated scores.

### RiskAssessmentService

Computes the overall HRT risk profile for a patient by evaluating medical history, family history, current medications, and clinical measurements. The service applies evidence-based risk models to quantify thromboembolism risk, cardiovascular risk, and cancer risk, producing a RiskFactorProfile value object. This calculation spans multiple data sources and applies complex clinical logic that does not belong to any single entity.

## Hormone Protocol Context Services

### ProtocolSelectionService

Recommends treatment protocols based on clinical assessment results and patient preferences. This service takes an AssessmentCompleted event payload and matches it against the FormulationCatalog to identify appropriate hormone combinations, delivery methods, and dosing ranges. It applies clinical guidelines to rank protocol options by appropriateness, producing a prioritized list of TreatmentProtocol drafts for prescriber review.

### DoseCalculationService

Calculates initial dosing for protocol components based on patient characteristics. This service considers age, weight, baseline hormone levels, delivery method pharmacokinetics, and clinical guidelines to determine starting doses. It encapsulates the pharmacological knowledge that translates patient parameters into specific Dosage value objects, applying safety constraints and evidence-based dosing algorithms.

### ProtocolCompatibilityService

Validates that a proposed set of protocol components is pharmacologically compatible. This service checks for hormone interactions, delivery method conflicts, and contraindicated combinations. It references the current ContraindicationRegistry through a cross-context query to ensure that proposed protocols do not include contraindicated elements for the specific patient.

## Lab Monitoring Context Services

### MonitoringScheduleService

Generates and adjusts monitoring schedules based on treatment protocol requirements, patient risk level, and treatment phase. This service determines which laboratory tests are needed, at what frequency, and with what therapeutic target ranges. It produces MonitoringFrequency value objects and ScheduledTest entities based on clinical guidelines and the current protocol state.

### TrendAnalysisService

Analyzes sequential laboratory results to identify trends in hormone levels and metabolic markers. This service takes a series of LabResult value objects for a specific test type and computes trend direction, rate of change, and stability assessment. It produces TrendAnalysis value objects that inform treatment optimization decisions. The service applies statistical methods appropriate for clinical time-series data.

### ThresholdEvaluationService

Evaluates whether a new laboratory result triggers threshold-based alerts. This service compares a LabResult against the TherapeuticTargetRange, applying clinical significance rules that distinguish between minor exceedances and clinically meaningful deviations. It determines alert severity and urgency, producing ThresholdExceeded events when appropriate.

## Side Effect Management Context Services

### CausalityAssessmentService

Performs standardized causality assessment for adverse events. This service applies assessment algorithms such as the Naranjo scale or WHO-UMC system to evaluate the probability that an adverse event is caused by HRT. It takes the adverse event details, temporal relationship to treatment, dechallenge and rechallenge data, and known drug effects to produce a causality determination.

### RiskStratificationService

Calculates and updates patient risk stratification based on accumulated adverse event data, lab results, and clinical factors. This service applies risk models that categorize patients into risk tiers (low, moderate, high, very high), determining monitoring intensity and triggering risk-appropriate clinical responses.

## Treatment Optimization Context Services

### TitrationCalculationService

Calculates dose titration recommendations based on lab trends, side effect data, and outcomes measurements. This service applies titration algorithms that determine the direction, magnitude, and timing of dose adjustments. It produces DoseAdjustment value objects and generates OptimizationRecommendation entities with supporting evidence and clinical rationale.

### FormulationSwitchingService

Evaluates whether a patient would benefit from switching to a different hormone formulation or delivery method. This service considers current treatment response, side effect profile, patient adherence data, and available alternative formulations to produce switching recommendations with expected benefit assessments.

## Outcomes Tracking Context Services

### EfficacyCalculationService

Calculates treatment efficacy metrics by comparing current outcome measurements against baseline values and treatment goals. This service produces SymptomResolutionRate and EfficacyRating value objects, applying statistical methods to account for measurement variability and confounding factors.

### OutcomeBenchmarkingService

Compares individual patient outcomes against population benchmarks and evidence-based expected outcomes. This service contextualizes a patient's treatment response within broader clinical evidence, identifying cases where outcomes are significantly above or below expectations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on domain services.
