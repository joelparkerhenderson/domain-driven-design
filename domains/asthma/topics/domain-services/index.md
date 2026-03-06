# Domain Services - Asthma Domain

## Overview

Domain services are stateless operations that encapsulate domain logic not naturally belonging to any single entity or value object. When an operation spans multiple aggregates or requires coordination between different domain concepts, a domain service provides a clear home for that logic. In the asthma management domain, domain services handle clinical decision support, cross-aggregate calculations, and policy enforcement that involve multiple domain objects.

Domain services are distinct from application services (which orchestrate use cases) and infrastructure services (which handle technical concerns). A domain service operates entirely in terms of domain concepts and ubiquitous language. It has no knowledge of databases, HTTP requests, or user interfaces.

## Domain Service Design Principles

**Statelessness.** Domain services do not hold state between operations. All necessary data is passed in as parameters or retrieved through repositories within the service method. This makes domain services inherently thread-safe and testable.

**Domain Language.** Domain service methods are named using ubiquitous language. A service method is called ClassifyAsthmaSeverity, not ProcessSeverityData. Method parameters and return types are domain objects (entities, value objects, aggregates).

**Cross-Aggregate Coordination.** Domain services coordinate work across multiple aggregates without violating aggregate boundaries. The service reads from aggregates, applies domain logic, and returns results or triggers domain events.

**Policy Encapsulation.** Clinical policies and business rules that span multiple concepts are encapsulated in domain services. The logic for determining biologic agent eligibility involves treatment history, biomarker values, and assessment results from multiple aggregates.

## Clinical Assessment Context Services

### AsthmaClassificationService

**Purpose:** Classify asthma severity and control level based on clinical assessment data.

**Operations:**

- **classifySeverity(spirometryResults, symptomFrequency, exacerbationHistory, nightSymptoms):** Applies GINA severity classification criteria to determine whether asthma is intermittent, mild persistent, moderate persistent, or severe persistent. Considers both impairment and risk domains.

- **assessControlLevel(symptomData, relieverUse, activityLimitation, lungFunctionData):** Evaluates current asthma control using GINA assessment criteria. Counts the number of uncontrolled criteria to classify as well-controlled, partly controlled, or uncontrolled.

- **evaluateBronchodilatorResponse(preBDFEV1, postBDFEV1):** Calculates percentage improvement in FEV1 after bronchodilator administration. A positive response (greater than or equal to 12% and 200mL improvement) supports asthma diagnosis.

### PhenotypingService

**Purpose:** Determine asthma phenotype and endotype based on clinical and biomarker data.

**Operations:**

- **determinePhenotype(clinicalHistory, allergyStatus, onsetAge, triggers, biomarkers):** Classifies asthma phenotype (allergic, exercise-induced, aspirin-exacerbated, late-onset, obesity-related) based on clinical presentation pattern.

- **classifyEndotype(eosinophilCount, fenoValue, igeLevel, neutrophilCount):** Determines whether the underlying mechanism is T2-high (eosinophilic) or T2-low (non-eosinophilic, neutrophilic, or paucigranulocytic). This classification directs therapy selection, particularly for biologic agents.

## Treatment Management Context Services

### StepwiseTherapyService

**Purpose:** Implement GINA stepwise therapy logic for treatment plan adjustments.

**Operations:**

- **evaluateStepUp(currentStep, controlLevel, adherenceRate, inhalerTechniqueScore):** Determines whether a step-up is warranted based on GINA criteria. Before recommending step-up, verifies that modifiable factors (poor adherence, incorrect inhaler technique) have been addressed. Returns a step-up recommendation with clinical rationale.

- **evaluateStepDown(currentStep, controlLevel, controlDuration, exacerbationHistory):** Determines whether a step-down is appropriate. Requires well-controlled asthma for at least 3 months with no exacerbations. Returns a step-down recommendation with safety conditions.

- **recommendTherapy(step, phenotype, patientPreferences):** Recommends specific medications for a given GINA step, considering the patient's phenotype and preferences. Returns preferred and alternative therapy options.

### BiologicEligibilityService

**Purpose:** Evaluate patient eligibility for biologic agent therapy.

**Operations:**

- **assessEligibility(treatmentHistory, biomarkers, exacerbationCount, ocsUsage):** Evaluates whether a patient meets criteria for biologic therapy initiation. Requires failure of optimized Step 4 therapy, specific biomarker thresholds (eosinophils >=150 cells/uL, or FeNO >=20 ppb, or IgE within range for omalizumab), and recurrent exacerbations or OCS dependence.

- **selectAgent(eligibilityCriteria, phenotype, endotype, comorbidities):** Recommends specific biologic agents based on the patient's immunological profile. Considers anti-IgE (omalizumab), anti-IL5 (mepolizumab, benralizumab), anti-IL4/13 (dupilumab), and anti-TSLP (tezepelumab) options.

## Trigger Monitoring Context Services

### TriggerCorrelationService

**Purpose:** Analyze correlations between trigger exposures and asthma exacerbations.

**Operations:**

- **correlateTriggersWithExacerbations(triggerProfile, exposureHistory, exacerbationHistory):** Analyzes temporal correlations between recorded trigger exposures and subsequent exacerbation events. Returns correlation strength and confidence level for each trigger type.

- **assessExposureRisk(triggerProfile, currentEnvironmentalData):** Evaluates current environmental conditions against a patient's trigger profile to determine real-time risk level. Returns risk classification (low, moderate, high) with specific trigger alerts.

## Action Planning Context Services

### ActionPlanGenerationService

**Purpose:** Generate personalized asthma action plans based on clinical data.

**Operations:**

- **generateZoneThresholds(personalBestPEF, treatmentStep):** Calculates zone thresholds based on the patient's personal best PEF. Green zone is typically 80-100% of personal best, yellow zone is 50-80%, and red zone is below 50%.

- **generateMedicationInstructions(treatmentPlan, zoneDefinitions):** Translates the treatment plan medications into patient-facing instructions for each zone. Green zone instructions describe daily controller medication use. Yellow zone instructions describe rescue medication escalation. Red zone instructions describe emergency medication dosing and when to call emergency services.

- **validateActionPlan(actionPlan, treatmentPlan, latestAssessment):** Validates that an action plan is consistent with the current treatment plan and latest clinical assessment. Detects discrepancies such as outdated medication names, zone thresholds inconsistent with personal best, or missing emergency contacts.

## Medication Management Context Services

### AdherenceAnalysisService

**Purpose:** Analyze medication adherence patterns and identify intervention needs.

**Operations:**

- **calculateAdherenceRate(prescriptionHistory, refillHistory, monitoringData, period):** Calculates medication adherence rate using available data sources (pharmacy refill records, electronic inhaler monitoring, patient self-report). Applies the most reliable available measurement method.

- **identifyAdherenceBarriers(adherencePattern, patientHistory, medicationRegimen):** Analyzes adherence patterns to identify potential barriers (forgetfulness, side effects, cost, complexity of regimen, health literacy). Returns categorized barrier assessment to guide intervention.

### RescueInhalerMonitoringService

**Purpose:** Monitor rescue inhaler usage patterns as an indicator of asthma control.

**Operations:**

- **assessRescueUsage(rescueInhalerEvents, period):** Calculates rescue inhaler usage frequency over a defined period. GINA guidelines consider use more than twice per week (excluding pre-exercise use) as an indicator of inadequate control.

- **detectUsageEscalation(usageHistory, baselineUsage):** Detects escalating rescue inhaler use that may signal worsening control. Compares recent usage patterns against the patient's baseline to identify statistically significant increases.

## Outcomes Tracking Context Services

### OutcomeTrendAnalysisService

**Purpose:** Analyze longitudinal outcome trends for clinical decision support.

**Operations:**

- **analyzeLungFunctionTrend(spirometryHistory, peakFlowHistory, period):** Calculates lung function trajectory (FEV1 trend, PEF trend) over a specified period. Identifies stable, improving, or declining patterns and calculates rate of change.

- **assessTreatmentEffectiveness(treatmentPlan, outcomeData, period):** Evaluates whether a treatment plan is achieving its goals by analyzing ACT score trends, exacerbation frequency, and lung function changes since treatment initiation or modification.

- **generateOutcomeReport(patientId, reportPeriod):** Compiles a comprehensive outcome report including ACT trends, exacerbation summary, lung function trends, adherence rates, and quality of life scores for clinical review.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on domain services.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023. Treatment decision algorithms.
