# Medication Management Context - Asthma Domain

## Overview

The Medication Management Context is responsible for handling medication prescriptions, dosing schedules, adherence monitoring, rescue inhaler usage tracking, and pharmacy system integration. This bounded context owns the models for controller medication regimens, rescue inhaler prescriptions, refill tracking, electronic adherence monitoring, and prescription fulfillment workflows. It is classified as a generic subdomain because prescription management follows well-established healthcare IT patterns.

While classified as generic, the Medication Management Context plays a critical role in asthma outcomes. Research consistently shows that poor medication adherence is the single most common cause of poorly controlled asthma, with controller medication adherence rates averaging only 50% in clinical practice. This context must reliably track adherence patterns and generate actionable alerts.

## Key Concepts

**Controller Medications** are taken daily to reduce airway inflammation and prevent asthma symptoms. The primary controller is the inhaled corticosteroid (ICS), with options including beclomethasone, budesonide, ciclesonide, fluticasone, and mometasone. Combination controllers (ICS/LABA) include fluticasone/salmeterol, budesonide/formoterol, and mometasone/formoterol. Additional controllers include leukotriene receptor antagonists (montelukast, zafirlukast), long-acting muscarinic antagonists (tiotropium), and oral theophylline. Each controller has a specific dosing schedule that must be followed consistently.

**Rescue Inhalers** provide rapid relief of acute symptoms. Short-acting beta-agonists (SABAs) such as albuterol and levalbuterol are the standard rescue medications. Increasingly, low-dose ICS-formoterol (budesonide-formoterol) is used as both maintenance and reliever therapy (MART approach). Rescue inhaler usage frequency serves as an important indicator of asthma control -- GINA considers use more than twice per week (excluding pre-exercise use) as a marker of inadequate control.

**Adherence Monitoring** tracks whether patients take their prescribed medications as directed. Measurement methods include pharmacy refill data (medication possession ratio), electronic inhaler monitors (recording date and time of actuations), patient self-report (often unreliable, tending to overestimate adherence), and canister weight or dose counters. Adherence is classified as good (above 80%), partial (50-80%), or poor (below 50%).

**Prescription Management** handles the lifecycle of medication prescriptions from creation through fulfillment, refill, and discontinuation. Prescriptions are created when treatment plans are initiated or modified. Electronic prescribing transmits prescriptions to pharmacies using NCPDP SCRIPT standards. Refill tracking monitors medication supply and alerts when refills are due or overdue.

## Aggregates

### MedicationRegimen

The central aggregate representing a patient's complete active medication list. Contains all current prescriptions with dosing schedules, refill status, and device information. Enforces that duplicate prescriptions for the same medication are not active simultaneously and that controller medications have defined daily dosing schedules.

### Prescription

Encapsulates a single medication prescription with drug identification, dosing, quantity, refills authorized, prescribing clinician, and pharmacy assignment. Tracks prescription status through its lifecycle (active, discontinued, expired, superseded).

### AdherenceRecord

Aggregates adherence data for a patient over a measurement period. Contains missed dose events, refill gap records, and calculated adherence rates. Enforces that adherence rate calculations use the most reliable available measurement method and that rates fall within the 0-100% range.

## Entities

- **Prescription:** Identity by PrescriptionID. Lifecycle: created, transmitted, filled, refilled, discontinued, expired.
- **MissedDoseEvent:** Identity by EventID. Records a single missed dose with detection method and reported reason.
- **RefillRecord:** Identity by RefillID. Records each pharmacy fill/refill with date, quantity, and days supply.

## Value Objects

- **MedicationIdentifier:** Drug name, RxNorm code, NDC code, therapeutic class.
- **DosingSchedule:** Frequency, dose amount, dose units, administration route, timing instructions.
- **AdherenceRate:** Percentage (0-100), measurement period, measurement method, classification (good/partial/poor).
- **RefillStatus:** Last refill date, next expected refill date, refills remaining, days supply remaining.
- **InhalerDevice:** Device type (MDI, DPI, SMI, nebulizer), brand name, dose counter reading, expiration date.
- **PrescriptionStatus:** Current status (active, discontinued, expired) with status change date and reason.

## Domain Events Published

- **AdherenceAlertRaised:** Published when adherence falls below threshold or rescue inhaler use exceeds baseline. Consumed by Treatment Management (evaluates whether poor control is due to non-adherence vs. therapy inadequacy), Outcomes Tracking (records adherence data point), and Action Planning (may trigger patient outreach).
- **PrescriptionFilled:** Published when a pharmacy confirms dispensing. Consumed by Outcomes Tracking (tracks fill patterns) and internal adherence calculations.
- **PrescriptionDiscontinued:** Published when a medication is stopped. Consumed by Action Planning (updates medication instructions) and Outcomes Tracking (records medication change).
- **ExcessiveRescueUseDetected:** Published when rescue inhaler use exceeds GINA thresholds (more than 2 times per week). Consumed by Treatment Management and Outcomes Tracking.

## Domain Events Consumed

- **TreatmentPlanSteppedUp (from Treatment Management):** Triggers creation of new prescriptions for added medications.
- **TreatmentPlanSteppedDown (from Treatment Management):** Triggers discontinuation of removed medications.
- **BiologicTherapyInitiated (from Treatment Management):** Triggers creation of biologic agent prescription with specialty pharmacy routing.
- **ActionPlanActivated (from Action Planning):** Triggers verification that prescription medications match action plan medication instructions.

## Domain Services

- **AdherenceAnalysisService:** Calculates adherence rates from available data sources, identifies adherence barriers, and classifies adherence patterns.
- **RescueInhalerMonitoringService:** Monitors rescue inhaler usage frequency, detects escalating use patterns, and generates control adequacy alerts.
- **PrescriptionReconciliationService:** Compares active prescriptions against the current treatment plan and action plan to detect discrepancies (medications prescribed but not in treatment plan, treatment plan medications without active prescriptions).

## Integration Points

- **Downstream from Treatment Management:** Treatment plan changes drive prescription creation, modification, and discontinuation. Customer-supplier relationship.
- **Upstream to Outcomes Tracking:** Adherence data and fill patterns are consumed for longitudinal outcome analysis. Customer-supplier relationship.
- **ACL to Pharmacy Systems:** Prescriptions are transmitted using NCPDP SCRIPT standard. Fill confirmations and refill notifications are received and translated into domain objects.
- **ACL to Insurance/Payer Systems:** Prior authorization requests for biologic agents and specialty medications are translated to payer-specific formats.
- **Open Host Service:** Exposes a standardized API for pharmacy system integration including prescription transmission, fill confirmation, and formulary queries.

## Medication Safety Rules

1. LABA-containing prescriptions must verify concurrent active ICS prescription (never LABA monotherapy).
2. Biologic agent prescriptions must route to specialty pharmacy capable of cold chain management.
3. Prescription interactions are checked against known drug interaction databases.
4. Maximum daily doses must not exceed approved limits for the patient's age group.
5. Prescriptions for controlled substances follow DEA scheduling requirements.
6. Medication reconciliation is triggered at each treatment plan modification.

## Business Rules

1. Only one active prescription per medication per patient at any time.
2. Controller medications require a defined daily dosing schedule.
3. Prescription discontinuation must record a reason (treatment change, adverse effect, patient request, therapeutic completion).
4. Adherence alerts are generated when adherence rate drops below 80% over a 30-day measurement period.
5. Rescue inhaler usage alerts are generated when use exceeds twice per week (excluding documented pre-exercise use).
6. Refill reminders are generated when days supply remaining drops below 7 days and refills remain authorized.
7. Expired prescriptions cannot be refilled; a new prescription is required.
8. Prescription transmission to pharmacy must occur within 24 hours of clinician authorization.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023. Chapter on medication adherence.
- Boulet et al. Adherence to Asthma Medications. Current Opinion in Pulmonary Medicine, 2018.
- NCPDP SCRIPT Standard. https://www.ncpdp.org/
