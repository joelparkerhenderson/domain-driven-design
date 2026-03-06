# Respirology Standards

## Overview

Clinical standards and guidelines shape the domain model of the respirology system by defining how measurements are taken, how results are interpreted, how diseases are classified, and how treatments are selected. These standards inform value object design, aggregate invariants, domain service algorithms, and published languages. In Domain-Driven Design, standards function as a form of published language -- an externally defined, well-documented vocabulary and ruleset that the domain model must incorporate.

The respirology domain incorporates standards from several professional organizations, each governing different aspects of respiratory medicine.

## American Thoracic Society / European Respiratory Society (ATS/ERS)

### Spirometry Standardization

The ATS/ERS standardization of spirometry defines the technical requirements for pulmonary function testing, including equipment specifications, patient preparation, test performance criteria, acceptability criteria, and repeatability criteria. These standards directly shape the domain model:

- The **TestQualityGrade** value object implements ATS/ERS quality grading (A through F) based on the number of acceptable maneuvers and the variability between the best two results.
- The **SpirometryResult** value object validates that FEV1 and FVC values are derived from acceptable maneuvers meeting ATS/ERS criteria.
- The **SpirometryInterpretationService** applies ATS/ERS interpretation algorithms, using lower limits of normal (LLN) from reference equations (GLI-2012) rather than fixed ratio thresholds for diagnosing airflow obstruction.

### Interpretation Strategies

The 2022 ERS/ATS technical standard on interpretive strategies provides the framework for the Respiratory Diagnostics Context's interpretation logic. It defines the decision tree for classifying spirometric patterns (normal, obstructive, restrictive, mixed, nonspecific) and grading severity using percent predicted FEV1.

### Pulmonary Rehabilitation Standards

The ATS/ERS statement on pulmonary rehabilitation defines the components, duration, and outcome measures for rehabilitation programs. These standards inform the Rehabilitation Program aggregate's invariants:

- Programs must include both exercise training and education components.
- Exercise prescription must be individualized based on functional assessment.
- Outcome measurement must include the 6MWT or shuttle walk test with standardized protocols.
- The minimal clinically important difference (MCID) for 6MWT distance (approximately 30 meters for COPD) is encoded in the **OutcomeMeasurementService**.

## Global Initiative for Chronic Obstructive Lung Disease (GOLD)

The GOLD framework provides the classification and treatment algorithm for COPD management. It directly shapes the Chronic Disease Management Context:

- The **GOLDClassification** value object encodes the spirometric severity stage (GOLD 1-4 based on FEV1 percent predicted) and the symptom/exacerbation group (A-D based on mMRC/CAT score and exacerbation history).
- The **GOLDClassificationService** implements the GOLD algorithm for combining spirometric and clinical data into a classification.
- Care Plan aggregate invariants ensure that pharmacotherapy recommendations align with the GOLD treatment algorithm: Group A patients receive a short-acting bronchodilator, Group B patients receive a long-acting bronchodilator, Group C patients receive a LAMA, and Group D patients receive LAMA+LABA or LAMA+ICS/LABA.
- The GOLD ABE assessment tool (updated from ABCD in GOLD 2023) is reflected in the classification algorithm.

## British Thoracic Society (BTS)

### Non-Invasive Ventilation Guidelines

BTS/ICS guidelines for NIV in acute hypercapnic respiratory failure inform the Ventilatory Support Context:

- Ventilator Configuration aggregate invariants enforce starting IPAP and EPAP ranges recommended by BTS.
- Escalation protocols follow BTS recommendations for adjusting pressures based on arterial blood gas response.
- The weaning and discontinuation criteria reflect BTS guidance.

### Bronchiectasis Guidelines

BTS guidelines for bronchiectasis management inform the Chronic Disease Management Context:

- Airway clearance technique selection algorithms in the AirwayClearanceProtocolService incorporate BTS recommendations.
- Exacerbation management protocols follow BTS antibiotic and escalation guidance.
- Long-term macrolide therapy criteria for exacerbation prevention reflect BTS recommendations.

### Oxygen Therapy Guidelines

BTS guidelines for emergency and home oxygen therapy inform both the Ventilatory Support and Chronic Disease Management contexts:

- Target SpO2 ranges (94-98% for most patients, 88-92% for those at risk of hypercapnia) are encoded as value object constraints.
- Home oxygen qualification criteria (PaO2 at or below 55 mmHg, or at or below 60 mmHg with secondary polycythemia, cor pulmonale, or nocturnal hypoxemia) inform the oxygen prescription logic.

## ARDS Network Protocols

The ARDS Network ventilation protocol informs the Ventilatory Support Context:

- Tidal volume limits (6 mL/kg predicted body weight, maximum 8 mL/kg) are enforced by the VentilatorParameterValidationService.
- PEEP-FiO2 combination tables (low-PEEP and high-PEEP arms) are implemented as validation rules.
- Plateau pressure targets (at or below 30 cm H2O) are enforced by aggregate invariants.

## Global Lung Function Initiative (GLI)

GLI-2012 reference equations provide the predicted values and lower limits of normal for spirometry across different demographic groups. These equations are used by the SpirometryInterpretationService to calculate percent predicted values and determine whether results fall below the lower limit of normal.

## Standards Governance in the Domain Model

Clinical standards are updated periodically. The domain model must accommodate standard updates without requiring wholesale redesign:

- Standard-dependent algorithms are encapsulated in domain services that can be updated independently.
- Value objects that encode standard-based classifications include version or effective date metadata.
- Configuration mechanisms allow switching between standard versions during transition periods.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Graham, B. L., et al. (2019). *Standardization of Spirometry 2019 Update*. AJRCCM.
- Global Initiative for Chronic Obstructive Lung Disease. (2024). *GOLD Report*.
- British Thoracic Society. (2019). *BTS/ICS Guideline for the Ventilatory Management of Acute Hypercapnic Respiratory Failure in Adults*. Thorax.
- Quanjer, P. H., et al. (2012). *Multi-Ethnic Reference Values for Spirometry (GLI-2012)*. European Respiratory Journal.
