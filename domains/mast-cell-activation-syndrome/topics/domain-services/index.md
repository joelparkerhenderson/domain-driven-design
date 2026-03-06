# Domain Services for Mast Cell Activation Syndrome

## Overview

Domain services are stateless operations that encapsulate business logic which does not naturally belong to any single entity or value object. They represent domain concepts that involve coordination across multiple aggregates or that implement logic spanning entity boundaries. In the MCAS domain, domain services handle clinical reasoning processes, cross-aggregate validations, and complex computations that require data from multiple sources.

Domain services are distinct from application services and infrastructure services. They contain pure domain logic expressed in the ubiquitous language. They do not handle transactions, security, or persistence concerns. Domain services are invoked by application services, which manage the orchestration of infrastructure and domain interactions.

## Diagnostic Assessment Services

### ConsensusCriteriaEvaluationService

The ConsensusCriteriaEvaluationService evaluates a patient's diagnostic data against the MCAS consensus criteria. This service spans the Diagnostic Workup and Bone Marrow Assessment aggregates, combining mediator levels, symptom evidence, and treatment response data to produce a Consensus Criteria Result value object.

The service implements the diagnostic algorithm that requires: documentation of episodic multi-system symptoms consistent with mast cell mediator release, laboratory evidence of mediator elevation above patient-specific baselines during symptomatic episodes, and demonstration of clinical response to medications targeting mast cell mediators. The service encapsulates the clinical reasoning process without modifying any aggregate directly.

### BaselineCalculationService

The BaselineCalculationService computes a patient's baseline mediator levels from historical measurements. It accepts a collection of Mediator Level value objects and applies statistical methods to determine the patient's personal baseline. This baseline is critical for evaluating whether acute elevations meet diagnostic thresholds. The service handles edge cases such as insufficient data for reliable baseline calculation and identifies outliers that may skew the baseline estimate.

## Trigger Management Services

### TriggerCorrelationService

The TriggerCorrelationService analyzes the temporal relationship between exposure events and symptom flares to identify potential triggers. It accepts symptom entries from the Symptom Tracking Context and exposure records from the Trigger Management Context, then evaluates whether the timing, frequency, and consistency of symptom onset following exposures meets the threshold for trigger identification.

The service applies correlation algorithms that account for variable reaction windows across different trigger types. Environmental triggers may produce symptoms within minutes, while dietary triggers may have delayed onset of hours or days. The service produces a correlation assessment that informs the confidence level assigned to a suspected trigger.

### AvoidancePlanValidationService

The AvoidancePlanValidationService reviews a proposed avoidance plan for internal consistency and completeness. It checks that environmental controls do not conflict with one another, that dietary restrictions maintain nutritional adequacy, and that the plan addresses all confirmed triggers in the patient's trigger profile. The service also identifies gaps where confirmed triggers lack corresponding avoidance strategies.

## Medication Protocol Services

### InteractionCheckService

The InteractionCheckService evaluates a proposed medication change against the patient's current regimen for potential drug interactions. It accepts the current Medication Regimen aggregate and the proposed change, then identifies interactions that may affect safety or efficacy. The service categorizes interactions by severity: contraindicated, serious, moderate, or minor.

For MCAS patients, the service also checks whether proposed medications contain known mast cell triggers or excipients that the patient has identified as problematic. This dual-check for pharmacological interactions and tolerability concerns is specific to the MCAS domain.

### TitrationPlanGenerationService

The TitrationPlanGenerationService creates a titration schedule for a new medication based on the medication type, the patient's sensitivity profile, and clinical guidelines. MCAS patients often require slower titration than standard protocols due to heightened sensitivity to medication changes. The service produces a sequence of Titration Step value objects tailored to the individual patient's tolerability history.

## Symptom Tracking Services

### PatternAnalysisService

The PatternAnalysisService examines longitudinal symptom data to identify recurring patterns. It accepts a collection of symptom entries and flare records, then applies pattern recognition to identify temporal cycles (time-of-day, day-of-week, seasonal patterns), organ system clustering (symptoms that tend to co-occur), and trend analysis (symptom burden increasing, stable, or decreasing over time).

The service produces a Pattern Analysis result that informs clinical decision-making in the Trigger Management and Medication Protocol contexts. Pattern identification may reveal unrecognized triggers or indicate that a medication adjustment is needed.

### SeverityScoringService

The SeverityScoringService computes aggregate severity scores from individual symptom entries. It accepts a collection of symptom entries for a defined period and produces a Severity Score that accounts for symptom frequency, intensity, duration, and multi-system involvement. The scoring algorithm weights symptoms by their impact on daily functioning and quality of life.

## Specialist Coordination Services

### CareTeamAssemblyService

The CareTeamAssemblyService recommends specialist involvement based on a patient's symptom profile and diagnostic results. It accepts the patient's confirmed diagnosis, their organ system involvement pattern, and the current care team composition, then identifies specialty gaps. For example, a patient with significant gastrointestinal symptoms but no gastroenterologist on the care team would receive a recommendation for that specialty.

### CarePlanReconciliationService

The CarePlanReconciliationService harmonizes treatment recommendations from multiple specialists into a coherent shared care plan. It identifies conflicts between specialist recommendations, flags potential medication interactions across prescribers, and highlights areas where specialist opinions diverge. The service does not resolve conflicts but presents them clearly for clinical decision-making.

## Outcomes Measurement Services

### SymptomBurdenCalculationService

The SymptomBurdenCalculationService computes the overall symptom burden score from symptom tracking data. It aggregates severity scores across organ systems, factors in flare frequency and duration, and normalizes the result against validated scoring instruments. The service supports both point-in-time burden calculation and trend analysis over defined periods.

### TreatmentEffectivenessService

The TreatmentEffectivenessService evaluates the impact of specific medication changes on patient outcomes. It correlates medication start dates, titration events, and discontinuation events with changes in symptom burden scores and quality of life ratings. The service produces an effectiveness assessment for each medication that informs ongoing treatment decisions.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on domain services and business logic.
