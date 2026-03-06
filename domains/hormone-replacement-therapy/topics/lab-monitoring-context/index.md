# Lab Monitoring Context in the Hormone Replacement Therapy Domain

The Lab Monitoring Context is a bounded context responsible for managing laboratory test ordering, result capture, trend analysis, and threshold-based alerting throughout the hormone replacement therapy (HRT) treatment lifecycle. This context ensures that patients on active HRT protocols are monitored appropriately, with results evaluated against therapeutic targets and clinical safety parameters.

## Context Overview

Laboratory monitoring is essential for safe and effective HRT management. Hormone levels must be tracked to confirm that treatment achieves therapeutic targets without excessive dosing. Metabolic panels assess liver function, lipid profiles, and glucose metabolism for treatment safety. Bone density and cardiovascular markers evaluate long-term health impact. The Lab Monitoring Context owns this entire monitoring workflow, from schedule generation through result interpretation and alert propagation.

## Domain Model

### Aggregates

The MonitoringPlan aggregate is the primary aggregate root, tracking all scheduled and completed laboratory monitoring for a patient-protocol pair. It enforces the invariant that every protocol-required test type must have a corresponding scheduled test entry. The LabResultSet aggregate groups results from a single collection event for cross-test correlation analysis.

### Key Entities

The MonitoringPlan entity manages the complete monitoring lifecycle. The ScheduledTest entity represents a specific test scheduled for a specific date, tracking the lifecycle from scheduled through collected to resulted.

### Key Value Objects

TherapeuticTargetRange defines the clinically acceptable range for a hormone measurement during treatment. LabResult captures a single test result with its value, unit, and reference context. TrendAnalysis summarizes the direction and rate of change across sequential measurements. MonitoringFrequency specifies the interval between scheduled tests.

## Monitoring Schedules

### Baseline Monitoring

Before treatment initiation, baseline laboratory panels establish pre-treatment values. Typical baseline panels include a complete hormone panel (estradiol, progesterone, total and free testosterone, FSH, LH, DHEA-S, SHBG), comprehensive metabolic panel (liver function, renal function, glucose), complete blood count, lipid panel, thyroid function tests, and vitamin D level.

### Early Treatment Monitoring

During the first three to six months of treatment, monitoring frequency is increased to assess initial response and safety. Hormone levels are typically checked at 4-6 weeks after initiation and again at 3 months. Liver function and lipid panels are repeated at 3 months. For testosterone therapy, hematocrit is monitored at 3 and 6 months due to the risk of polycythemia.

### Maintenance Monitoring

After treatment stabilization, monitoring transitions to a maintenance schedule, typically every 6-12 months. The MonitoringScheduleService adjusts frequency based on treatment phase, risk level, and clinical response. Higher-risk patients or those undergoing dose titration receive more frequent monitoring.

### Specialized Monitoring

Certain clinical scenarios require additional monitoring. Bone density assessment via DEXA scan is recommended at baseline and every 1-2 years for patients at osteoporosis risk. Cardiovascular markers including high-sensitivity C-reactive protein and homocysteine may be monitored in patients with cardiovascular risk factors. Endometrial assessment may be required for patients on estrogen therapy with an intact uterus.

## Therapeutic Target Ranges

The context maintains clinically defined therapeutic target ranges distinct from standard laboratory reference ranges. For example, the reference range for estradiol may be 15-350 pg/mL for premenopausal women, but the therapeutic target for menopausal HRT might be 50-200 pg/mL. For testosterone replacement in men, the therapeutic target typically aims for 450-700 ng/dL mid-range, while the laboratory reference range extends to 1100 ng/dL. These target ranges are configured per protocol and may be adjusted based on individual patient response.

## Trend Analysis

The TrendAnalysisService evaluates sequential results to identify patterns that inform treatment optimization. Key analyses include trend direction assessment (are hormone levels rising, falling, or stable), rate of change calculation (how quickly levels are moving), stability evaluation (has the patient reached steady state), and cross-test correlation (do related markers move consistently, such as estradiol and FSH showing inverse trends as treatment takes effect).

## Threshold Alerting

The ThresholdEvaluationService generates alerts when results fall outside therapeutic target ranges. Alerts are classified by clinical urgency: informational (minor deviation within clinical tolerance), advisory (meaningful deviation warranting clinical review at next visit), urgent (significant deviation requiring prompt clinical attention), and critical (dangerous deviation requiring immediate clinical response). The service applies clinical significance rules to avoid alert fatigue from minor fluctuations.

## Domain Events

The context publishes LabResultRecorded when a result is captured and validated, ThresholdExceeded when a result falls outside the therapeutic target range, and MonitoringScheduleAdjusted when monitoring frequency or panel composition changes.

## Context Relationships

The Lab Monitoring Context is downstream to the Hormone Protocol Context, conforming to protocol-defined monitoring requirements. It publishes results and alerts to the Side Effect Management Context and Treatment Optimization Context through published language. It maintains an anti-corruption layer against external laboratory information systems for result integration.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Bhasin, S., et al. "Testosterone Therapy in Men with Hypogonadism." JCEM, 2018.
- Santen, R.J., et al. "Competency in Menopause Management." JCEM, 2010.
