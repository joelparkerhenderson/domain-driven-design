# Reporting & Visualization Context

The Reporting and Visualization Context is the bounded context responsible for generating trend reports, clinical dashboards, physician summaries, and patient portal views from cardiac health data. It aggregates information from multiple upstream contexts and formats it for diverse audiences with different information needs.

## Overview

The Reporting and Visualization Context transforms raw cardiac metrics, analysis findings, risk assessments, and alert histories into meaningful presentations for clinicians, patients, and administrators. It sits downstream of most other bounded contexts, consuming their outputs to produce comprehensive views of cardiac health status and trends.

This context is classified as a supporting subdomain. It does not contain novel clinical logic but is essential for delivering the value created by the analysis and risk assessment contexts to end users. Effective cardiac health reporting must present complex physiological data in ways that support clinical decision-making and patient engagement.

## Key Concepts

**Trend Reports.** Time-series summaries showing how cardiac metrics have changed over configurable periods (daily, weekly, monthly, quarterly). Trend reports present heart rate patterns, blood pressure trajectories, HRV evolution, and risk score changes. They highlight inflection points, seasonal patterns, and responses to interventions.

**Physician Summaries.** Concise clinical documents designed for physician consumption during patient encounters. They present the most recent cardiac metrics, any triggered alerts, current risk stratification, notable trends, and suggested follow-up actions. Physician summaries prioritize actionability and clinical relevance.

**Patient Portal Views.** Patient-facing presentations of cardiac health data designed for comprehension by non-clinical audiences. They use simplified language, contextual explanations (e.g., "your blood pressure is in the elevated range"), and goal-tracking features. Patient portal views support self-management and treatment adherence.

**Clinical Dashboards.** Real-time or near-real-time views of cardiac metrics across patient populations. Dashboards support monitoring teams who oversee multiple patients simultaneously, highlighting patients with active alerts, deteriorating trends, or pending risk reassessments.

**Report Templates.** Configurable templates that define the structure, content selection, and formatting rules for different report types. Templates can be customized for institutional preferences, specialty-specific views (cardiology versus primary care), and regulatory reporting requirements.

**Data Aggregation.** The context aggregates data across time periods, metric types, and patient populations. Aggregation logic produces statistical summaries (means, medians, standard deviations, percentiles) from raw measurement series while preserving the ability to drill down into individual readings.

## Domain Examples

A cardiologist opening a patient's quarterly cardiac health report sees a trend chart showing blood pressure readings over 90 days, with a downward trajectory from Stage 2 hypertension to elevated blood pressure following a medication change. The report highlights the medication change date, the rate of improvement, and the current ASCVD risk score reduction. The physician summary recommends continuing current therapy and reassessing in 3 months.

A patient logs into the patient portal and sees their weekly heart rate summary: average resting heart rate of 72 bpm (down from 78 bpm last month), HRV trending upward (indicating improved autonomic function), and a congratulatory note that their cardiovascular risk category improved from intermediate to borderline. The portal provides context-appropriate explanations and links to educational resources.

## Relationships to Other Topics

- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - Provides analysis results for trend reporting.
- [Risk Assessment Context](../risk-assessment-context/index.md) - Provides risk scores for dashboard display.
- [Monitoring & Alerts Context](../monitoring-alerts-context/index.md) - Provides alert history for reporting.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Reports may be exported as FHIR resources.
- [Value Objects](../value-objects/index.md) - Reports present aggregated value objects.
- [Subdomains](../subdomains/index.md) - Classified as a supporting subdomain.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- Few, Stephen. *Information Dashboard Design*. Analytics Press, 2013.
- Tufte, Edward. *The Visual Display of Quantitative Information*. Graphics Press, 2001.
- Shneiderman, B. et al. "Improving Healthcare with Interactive Visualization." *Computer*, 46(5), 58-66, 2013.
