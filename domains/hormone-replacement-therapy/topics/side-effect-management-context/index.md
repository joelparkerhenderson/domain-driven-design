# Side Effect Management Context in the Hormone Replacement Therapy Domain

The Side Effect Management Context is a bounded context responsible for tracking adverse events associated with hormone replacement therapy (HRT), maintaining contraindication databases, performing risk stratification, and coordinating mitigation strategies. This context ensures patient safety by systematically identifying, evaluating, and responding to treatment-related complications throughout the HRT lifecycle.

## Context Overview

Hormone replacement therapy carries specific risks that require systematic monitoring and management. Estrogen therapy may increase thromboembolism risk. Testosterone therapy may cause polycythemia or liver enzyme elevation. Progesterone may cause mood changes or breakthrough bleeding. The Side Effect Management Context owns the pharmacovigilance workflow, from adverse event detection through causality assessment, mitigation implementation, and regulatory reporting.

## Domain Model

### Aggregates

The AdverseEventRecord aggregate is the primary aggregate root, capturing a single adverse event occurrence with its assessment history and mitigation actions. It enforces the invariant that resolved events must have a resolution date and documented outcome. The ContraindicationRegistry aggregate maintains the current set of absolute and relative contraindications, ensuring that no condition is simultaneously classified as both absolute and relative for the same hormone type.

### Key Entities

The AdverseEventRecord entity tracks the event lifecycle from initial report through investigation to resolution. The CausalityAssessment entity records formal evaluations of the event-treatment relationship. The MitigationAction entity documents interventions taken to address the event.

### Key Value Objects

SeverityGrade classifies event severity using standardized scales such as CTCAE. EventClassification categorizes events using MedDRA terminology for standardized reporting. ResolutionStatus tracks the current state of event resolution.

## Adverse Event Categories

### Estrogen-Related Adverse Events

Common estrogen-related adverse events include venous thromboembolism (deep vein thrombosis, pulmonary embolism), breast tenderness or swelling, headaches, nausea, bloating, mood changes, and vaginal bleeding. Serious events include stroke, myocardial infarction, and endometrial hyperplasia. Risk factors include age over 60, obesity, smoking history, personal or family history of thromboembolism, and factor V Leiden or other thrombophilias.

### Progesterone-Related Adverse Events

Common progesterone-related adverse events include mood changes (depression, irritability), breast tenderness, bloating, headaches, fatigue, and breakthrough bleeding. The specific side effect profile varies between micronized progesterone and synthetic progestins, with micronized progesterone generally showing a more favorable profile based on data from the WHI and French E3N studies.

### Testosterone-Related Adverse Events

Common testosterone-related adverse events include acne, hair growth changes (hirsutism in women, acceleration of male pattern baldness), mood changes (irritability, aggression), polycythemia, liver enzyme elevation, sleep apnea, and cardiovascular effects. In men, additional concerns include prostate-specific antigen elevation, testicular atrophy, and fertility suppression. Hematocrit monitoring is essential because polycythemia represents a dose-dependent and potentially serious complication.

## Causality Assessment

The CausalityAssessmentService applies standardized assessment methodologies to evaluate the relationship between an adverse event and HRT. The Naranjo Adverse Drug Reaction Probability Scale uses a structured questionnaire to classify causality as definite, probable, possible, or doubtful. The WHO-UMC system provides an alternative classification framework. Assessment considers temporal relationship, dechallenge (did the event improve when treatment was stopped), rechallenge (did the event recur when treatment was restarted), known pharmacological effects, and alternative explanations.

## Risk Stratification

The RiskStratificationService categorizes patients into risk tiers based on accumulated clinical data. Low-risk patients have no identified risk factors and no adverse events. Moderate-risk patients have relative contraindications or minor adverse events. High-risk patients have multiple risk factors, significant adverse events, or relative contraindications requiring enhanced monitoring. Very-high-risk patients have conditions approaching absolute contraindication where treatment continuation requires exceptional clinical justification.

## Mitigation Strategies

Mitigation strategies follow a tiered approach. First-tier mitigations include dose reduction, administration timing adjustment, and supportive measures (such as antiemetics for nausea). Second-tier mitigations include formulation switching (such as transdermal instead of oral to reduce hepatic first-pass effects and thromboembolism risk) and addition of protective agents. Third-tier mitigations include treatment suspension or discontinuation when risks outweigh benefits.

## Contraindication Management

The ContraindicationRegistry maintains current absolute and relative contraindications informed by clinical guidelines. Absolute contraindications include conditions where HRT is never appropriate. Relative contraindications represent conditions where HRT may be used with enhanced monitoring and risk mitigation. The registry is updated as new clinical evidence emerges, and changes trigger review of active treatment protocols for affected patients.

## Domain Events

The context publishes SideEffectReported when a new adverse event is documented, RiskLevelChanged when patient risk stratification is updated, and ContraindicationDetected when a new contraindication is identified for a patient on active treatment.

## Context Relationships

The Side Effect Management Context consumes events from Lab Monitoring (threshold exceedances) and Clinical Assessment (risk factor updates). It shares a risk factor kernel with the Clinical Assessment Context. It publishes events consumed by Treatment Optimization and Hormone Protocol contexts. It maintains an anti-corruption layer against external regulatory reporting systems such as FDA MedWatch.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Rossouw, J.E., et al. "Risks and Benefits of Estrogen Plus Progestin." JAMA, 2002. (WHI Study)
- Naranjo, C.A., et al. "A Method for Estimating the Probability of Adverse Drug Reactions." Clinical Pharmacology & Therapeutics, 1981.
