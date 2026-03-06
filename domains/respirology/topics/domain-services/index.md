# Domain Services in the Respirology Domain

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to a single entity or value object. Domain services arise when an operation spans multiple aggregates, requires coordination between different domain concepts, or represents a pure domain computation that has no home in any particular aggregate. In the respirology domain, domain services model clinical algorithms, cross-aggregate coordination, and complex business rules that involve data from multiple sources.

Domain services are distinct from application services (which orchestrate use cases) and infrastructure services (which handle technical concerns like persistence and messaging). Domain services contain pure business logic expressed in the ubiquitous language.

## Domain Service Characteristics

### Statelessness

Domain services do not hold state between invocations. They receive all required inputs as parameters and return results without maintaining any internal memory. This statelessness makes domain services easy to test, reason about, and scale.

### Domain Language

Domain services are named using the ubiquitous language and their methods describe domain operations. A service named `RiskStratificationService` with a method `stratifyRespiratoryRisk(assessment, diagnosticHistory)` communicates its purpose clearly to domain experts and developers.

### Cross-Aggregate Operations

Domain services handle operations that span aggregate boundaries. When an operation requires data from multiple aggregates to produce a result, a domain service coordinates the computation without violating aggregate encapsulation.

## Key Domain Services by Bounded Context

### Clinical Assessment Context

**RiskStratificationService**: Computes a patient's respiratory risk level based on assessment findings, symptom scores, vital signs, and historical data. This service combines data from the current Respiratory Assessment aggregate with historical assessment data to produce a RiskLevel value object. The algorithm considers mMRC grade, CAT score, SpO2 trends, exacerbation frequency, and comorbidity burden.

**SymptomTrendAnalysisService**: Analyzes longitudinal symptom data across multiple assessments to identify deterioration patterns, improvement trends, or stability. This service takes a series of SymptomScore value objects and produces a TrendAnalysis result indicating the direction and magnitude of change.

### Respiratory Diagnostics Context

**SpirometerInterpretationService**: Interprets spirometry results by comparing measured values against predicted values based on patient demographics (age, height, sex, ethnicity), applying ATS/ERS interpretation algorithms. This service takes raw spirometry measurements and patient reference data and produces an interpreted SpirometryResult with classification (normal, obstructive, restrictive, mixed) and severity grading.

**DiagnosticCorrelationService**: Correlates results from multiple diagnostic tests (spirometry, imaging, ABG, sputum analysis) to produce an integrated diagnostic picture. This service identifies consistency or discrepancy between test results and flags findings that warrant further investigation.

### Airway Management Context

**AirwayClearanceProtocolService**: Selects the appropriate airway clearance technique and parameters based on the patient's condition, diagnosis, and current respiratory status. This service takes patient assessment data and disease information and produces an AirwayClearanceParameters value object specifying the recommended technique, duration, frequency, and positioning.

**BronchodilatorResponseAssessmentService**: Evaluates the patient's response to bronchodilator administration by comparing pre- and post-bronchodilator spirometry or peak flow measurements. This service determines whether the response meets criteria for significant reversibility, informing diagnosis and treatment decisions.

### Chronic Disease Management Context

**GOLDClassificationService**: Determines a COPD patient's GOLD classification by combining spirometric severity (stages I-IV based on FEV1 percent predicted) with symptom assessment (mMRC or CAT score) and exacerbation history (frequency and severity). This service takes spirometry results, symptom scores, and exacerbation records and produces a GOLDClassification value object.

**ExacerbationRiskPredictionService**: Estimates the probability of future exacerbations based on historical exacerbation patterns, current disease severity, medication adherence, seasonal factors, and comorbidities. This service produces a risk score and recommended preventive interventions.

**CarePlanReviewService**: Evaluates whether a care plan requires updating based on new diagnostic results, assessment changes, or elapsed time since last review. This service compares the current care plan's assumptions against current clinical data and produces a review recommendation.

### Ventilatory Support Context

**WeaningReadinessAssessmentService**: Evaluates whether a patient meets criteria for initiation of ventilator weaning. This service considers respiratory mechanics, gas exchange, hemodynamic stability, neurological status, and sedation level to produce a WeaningReadiness assessment with a recommendation and supporting rationale.

**VentilatorParameterValidationService**: Validates that a proposed set of ventilator parameter changes is clinically safe for the specific patient. This service checks tidal volume against predicted body weight, validates PEEP-FiO2 combinations against ARDS Network tables, and identifies potentially harmful parameter combinations.

### Pulmonary Rehabilitation Context

**ExercisePrescriptionService**: Generates an individualized exercise prescription based on the patient's functional assessment results (6MWT, cardiopulmonary exercise test), disease severity, comorbidities, and rehabilitation goals. This service produces an ExercisePrescription entity with modality-specific intensity, duration, and frequency targets.

**OutcomeMeasurementService**: Calculates rehabilitation outcome metrics by comparing baseline and current functional assessments, symptom scores, and quality-of-life measures. This service produces outcome reports that quantify improvement, stability, or decline across multiple domains.

**DischargeReadinessService**: Evaluates whether a patient has met the criteria for rehabilitation program discharge, including functional improvement targets, self-management competency, and maintenance plan completion. This service produces a discharge readiness assessment.

## Domain Service Design Guidelines

- Domain services should be used sparingly. If logic can belong to an aggregate or value object, it should not be extracted into a service.
- Domain services should depend only on domain types, not on infrastructure types.
- Domain services should be easily testable with unit tests using domain objects as inputs and outputs.
- Domain services should not publish domain events directly; event publication is the responsibility of aggregates.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 5 on domain services.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 7 on domain services.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9 on domain services and business logic organization.
