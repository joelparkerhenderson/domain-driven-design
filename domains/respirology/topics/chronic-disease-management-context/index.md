# Chronic Disease Management Context

## Overview

The Chronic Disease Management Context oversees long-term care plans for patients with chronic respiratory conditions. It handles COPD action plans with zone-based self-management instructions, interstitial lung disease (ILD) monitoring protocols, bronchiectasis management including exacerbation tracking, and sarcoidosis surveillance. This bounded context is classified as a core subdomain because it encodes the complex longitudinal clinical reasoning required to manage patients with progressive respiratory diseases over months and years.

This context consumes diagnostic results and clinical assessments from upstream contexts and produces care plan updates that influence the Pulmonary Rehabilitation Context's program design.

## Ubiquitous Language

Key terms within this context include: care plan, action plan, GOLD classification, GOLD stage, GOLD group, exacerbation, exacerbation severity, exacerbation frequency, treatment escalation, treatment de-escalation, self-management, zone (green/yellow/red), monitoring protocol, review date, disease progression, lung function decline rate, medication regimen, inhaler therapy, oxygen therapy, ILD monitoring, forced vital capacity trend, bronchiectasis exacerbation, sarcoidosis surveillance, and multidisciplinary team review.

## Aggregate: Care Plan

The Care Plan is the aggregate root, representing the comprehensive management plan for a patient's chronic respiratory condition.

**Components**:
- Care Plan ID (unique identifier)
- Patient ID (reference)
- Primary diagnosis (COPD, ILD, bronchiectasis, sarcoidosis, or other)
- Disease classification (GOLD classification for COPD, ILD severity, bronchiectasis severity index)
- Action plan (zone-based self-management instructions)
- Medication regimen (maintenance medications, rescue medications, escalation medications)
- Monitoring schedule (spirometry frequency, imaging frequency, laboratory tests, clinical review intervals)
- Exacerbation history (list of exacerbation records)
- Oxygen therapy prescription if applicable
- Care plan status (active, under review, suspended, archived)
- Created date, last reviewed date, next review date
- Managing clinician ID
- Multidisciplinary team members

**Invariants**:
- A COPD care plan must include an action plan with all three zones (green, yellow, red) defined.
- The medication regimen must be consistent with the current disease classification (e.g., GOLD D patients require combination therapy).
- The monitoring schedule must include spirometry at intervals appropriate to disease severity.
- A care plan cannot remain active beyond its next review date without review confirmation.
- Exacerbation records must be in chronological order and each must have a severity classification.

## Entities

**Action Plan**: Contains the zone-based instructions for patient self-management. The green zone describes baseline management when symptoms are stable. The yellow zone describes initial escalation steps when symptoms worsen. The red zone describes emergency actions when symptoms are severe. Each zone includes symptom criteria for entering the zone, medication adjustments, activity modifications, and criteria for seeking medical attention.

**Exacerbation Record**: Tracks individual exacerbation episodes including onset date, severity (mild, moderate, severe), triggering factors, treatment provided (increased bronchodilators, oral corticosteroids, antibiotics, hospitalization), duration, resolution date, and post-exacerbation assessment results.

**Monitoring Protocol**: Defines the scheduled monitoring activities tailored to the patient's condition, including specific test types, target frequencies, escalation triggers (e.g., FVC decline exceeding 10% in 12 months for ILD patients), and responsible clinicians.

## Value Objects

**GOLDClassification**: Captures the GOLD stage (I-IV based on FEV1 percent predicted), the symptom group (A-D based on mMRC/CAT and exacerbation history), and the classification date. This value object determines the recommended medication regimen and monitoring intensity.

**ExacerbationSeverity**: Classifies severity as mild (managed with short-acting bronchodilators only), moderate (requiring short course of oral corticosteroids and/or antibiotics), or severe (requiring emergency department visit or hospitalization).

**ActionPlanZone**: Represents a single zone with its entry criteria, medication instructions, activity guidance, and escalation triggers.

**LungFunctionTrend**: Captures the rate of FEV1 or FVC decline over a defined period, calculated from serial spirometry results. Used to detect accelerated disease progression.

**MedicationRegimen**: Lists the maintenance and rescue medications with drug names, doses, frequencies, and delivery devices. Includes inhaler technique requirements.

## Domain Events

**ExacerbationDetected**: Published when clinical criteria indicate a new exacerbation. Carries the patient ID, severity, triggering criteria, and recommended immediate actions. Consumed by the Clinical Assessment Context (urgent review needed) and Ventilatory Support Context (potential ventilatory needs).

**CarePlanUpdated**: Published when the care plan is revised. Carries the change summary, new classification if changed, and updated monitoring schedule. Consumed by the Pulmonary Rehabilitation Context.

**ActionPlanRevised**: Published when the self-management action plan is modified. Carries the updated zone definitions and medication instructions.

**DiseaseProgressionDetected**: Published when monitoring reveals significant disease progression such as accelerated FEV1 decline or worsening imaging findings. Triggers multidisciplinary team review.

**ReviewDueNotification**: Published when a care plan approaches or passes its scheduled review date. Triggers clinical workflow for care plan review.

## Domain Services

**GOLDClassificationService**: Computes GOLD classification from spirometry results, symptom scores, and exacerbation history per current GOLD guidelines.

**ExacerbationRiskPredictionService**: Estimates future exacerbation probability based on historical patterns, disease severity, medication adherence, seasonal factors, and environmental exposures.

**CarePlanReviewService**: Evaluates whether the current care plan remains appropriate given new clinical data, or whether modifications are indicated.

**LungFunctionDeclineService**: Calculates the rate of lung function decline from serial measurements and determines whether the rate exceeds expected age-related decline, suggesting disease progression.

## Disease-Specific Considerations

### COPD Management

COPD care plans follow the GOLD framework for pharmacotherapy escalation, starting with a single bronchodilator and stepping up to dual therapy, triple therapy, and consideration of roflumilast or macrolide prophylaxis based on exacerbation frequency and symptoms.

### ILD Monitoring

ILD monitoring protocols emphasize serial FVC measurement, DLCO trending, and HRCT surveillance. A decline in FVC of more than 5-10% over 6-12 months may indicate disease progression requiring treatment adjustment.

### Bronchiectasis Management

Bronchiectasis care plans focus on airway clearance adherence, sputum surveillance, exacerbation prevention, and antibiotic stewardship. Long-term macrolide therapy is considered for patients with frequent exacerbations.

### Sarcoidosis Surveillance

Sarcoidosis surveillance protocols include pulmonary function monitoring, chest imaging, serum ACE levels, and assessment for extrapulmonary manifestations. Treatment decisions depend on organ involvement and functional impairment.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Global Initiative for Chronic Obstructive Lung Disease (GOLD). (2024). *Global Strategy for COPD*.
- Raghu, G., et al. (2022). *Idiopathic Pulmonary Fibrosis: Evidence-based Guidelines*. American Journal of Respiratory and Critical Care Medicine.
