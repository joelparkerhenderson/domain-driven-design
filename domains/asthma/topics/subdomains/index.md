# Subdomains - Asthma Domain

## Overview

Subdomains classify areas of a domain by their strategic importance to the business. In Domain-Driven Design, subdomains are categorized as core, supporting, or generic. This classification guides investment decisions: core subdomains receive the most modeling effort and custom development, supporting subdomains enable core functions but follow less unique patterns, and generic subdomains can leverage existing solutions or frameworks.

In the asthma management domain, subdomain classification reflects the clinical reality that certain capabilities -- diagnostic assessment and evidence-based treatment planning -- represent the specialized expertise that differentiates an asthma management system, while other capabilities -- medication dispensing workflows, data reporting -- follow standardized healthcare patterns.

## Core Subdomains

Core subdomains embody the most valuable, differentiating capabilities of the system. They require deep domain expertise, custom modeling, and continuous refinement.

### Clinical Assessment Subdomain

Clinical assessment is a core subdomain because it encapsulates the specialized diagnostic logic that determines asthma severity and control status. The interpretation of spirometry results, the clinical significance of FeNO levels, and the application of GINA classification criteria require expert clinical knowledge that cannot be sourced from generic healthcare platforms.

Key complexities include:
- Interpreting FEV1/FVC ratios relative to predicted values adjusted for age, sex, height, and ethnicity.
- Correlating FeNO levels with eosinophilic inflammation to guide treatment phenotyping.
- Applying GINA assessment criteria that consider both impairment and future risk dimensions.
- Distinguishing asthma from conditions with overlapping symptoms (COPD, vocal cord dysfunction).

### Treatment Management Subdomain

Treatment management is a core subdomain because it implements the evidence-based stepwise therapy approach that is central to asthma care quality. The logic for step-up and step-down decisions, biologic agent eligibility determination, and phenotype-directed therapy selection requires specialized clinical decision support that represents significant competitive value.

Key complexities include:
- Applying GINA stepwise therapy rules with patient-specific modifications.
- Evaluating eligibility for biologic agents based on biomarker thresholds (IgE levels, eosinophil counts, FeNO).
- Managing combination therapy interactions (ICS/LABA, ICS/LABA/LAMA combinations).
- Supporting shared decision-making between clinicians and patients on treatment options.

## Supporting Subdomains

Supporting subdomains enable core functions but involve domain logic that, while specific to asthma management, follows recognizable patterns and does not require the same depth of custom modeling.

### Action Planning Subdomain

Action planning is a supporting subdomain because it translates core clinical decisions into patient-facing self-management instructions. The zone system (green, yellow, red) follows a well-established framework, but the personalization of zone thresholds, medication instructions, and emergency protocols requires domain-specific logic.

Key characteristics:
- The zone system pattern is standardized across asthma care (NAEPP guidelines).
- Personalization of zone thresholds depends on individual patient assessment data.
- Emergency protocols follow established clinical emergency response patterns.
- Patient communication requires health literacy considerations specific to asthma education.

### Trigger Monitoring Subdomain

Trigger monitoring is a supporting subdomain because it provides environmental context essential for clinical decision-making and patient self-management, but the monitoring patterns (sensor data collection, threshold alerting, correlation analysis) follow recognizable data processing patterns.

Key characteristics:
- Allergen tracking follows established allergology classifications.
- Air quality monitoring integrates with standardized external data sources (EPA AQI).
- Occupational exposure assessment follows occupational health standards (OSHA).
- Trigger-exacerbation correlation analysis uses standard epidemiological methods.

## Generic Subdomains

Generic subdomains provide necessary functionality but do not require domain-specific custom modeling. They can leverage off-the-shelf solutions, open-source libraries, or established healthcare standards.

### Medication Management Subdomain

Medication management is a generic subdomain because prescription workflows, medication databases, adherence tracking, and pharmacy integration follow well-established healthcare IT patterns. Standard drug databases (RxNorm, NDC), prescription standards (NCPDP SCRIPT), and medication reconciliation processes are applicable across all therapeutic areas, not just asthma.

Key characteristics:
- Drug information follows standardized terminologies (RxNorm, NDC codes).
- Prescription transmission uses established standards (NCPDP SCRIPT, HL7 FHIR MedicationRequest).
- Adherence monitoring follows general medication adherence patterns.
- Pharmacy integration uses industry-standard interfaces.

### Outcomes Tracking Subdomain (Partially Generic)

Outcomes tracking contains both generic and supporting elements. The infrastructure for recording questionnaire scores, tracking longitudinal trends, and generating reports is generic. However, the selection of asthma-specific outcome measures (ACT, AQLQ) and the clinical interpretation of outcome trends involves supporting-level domain logic.

Key characteristics:
- Patient-reported outcome measurement follows generic survey and scoring patterns.
- Longitudinal data aggregation and trend analysis use standard statistical methods.
- Asthma-specific outcome measures (ACT scoring, exacerbation classification) require domain knowledge.
- Population health reporting follows standard quality measurement frameworks (HEDIS).

## Investment Implications

| Subdomain              | Type       | Investment Level | Modeling Depth |
|------------------------|------------|------------------|----------------|
| Clinical Assessment    | Core       | Highest          | Deep custom    |
| Treatment Management   | Core       | Highest          | Deep custom    |
| Action Planning        | Supporting | Moderate         | Domain-specific|
| Trigger Monitoring     | Supporting | Moderate         | Domain-specific|
| Medication Management  | Generic    | Lower            | Standards-based|
| Outcomes Tracking      | Mixed      | Moderate         | Hybrid         |

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1 on subdomain classification.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
