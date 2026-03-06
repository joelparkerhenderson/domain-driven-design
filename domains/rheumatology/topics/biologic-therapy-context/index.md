# Biologic Therapy Context

## Overview

The Biologic Therapy Context governs the specialized workflow for prescribing, administering, and monitoring biologic disease-modifying antirheumatic drugs (bDMARDs) and targeted synthetic DMARDs (tsDMARDs). This context is separated from disease management because biologic therapy involves unique regulatory requirements, complex safety monitoring, infusion center logistics, biosimilar switching protocols, and pharmacovigilance obligations that warrant their own bounded context. It is a core subdomain given the clinical complexity, patient safety implications, and regulatory scrutiny involved.

## Context Boundaries

The Biologic Therapy Context owns prescribing workflows, pre-treatment screening, infusion management, safety monitoring, biosimilar switching, and adverse event documentation. It does not classify diseases (owned by the Autoimmune Disease Context), perform clinical assessments (owned by the Clinical Assessment Context), or manage non-biologic pain interventions (owned by the Pain Management Context).

The context receives therapy requests from the Autoimmune Disease Context and, less frequently, the Joint Disease Context. It returns therapy status updates and adverse event notifications. This separation ensures that disease management decisions remain in the clinical context while drug-specific logistics remain here.

## Aggregate: TherapyRegimen

The TherapyRegimen aggregate root manages the complete lifecycle of a biologic or targeted synthetic therapy for a patient. It contains BiologicPrescription entities, PreTreatmentScreening value objects, InfusionSession entities (for IV biologics), SafetyMonitoring entities, and BiosimilarSwitch entities.

The aggregate enforces critical safety invariants. Therapy cannot be initiated without completed pre-treatment screening. Screening includes tuberculosis evaluation (QuantiFERON-TB Gold or tuberculin skin test), hepatitis B surface antigen and core antibody, hepatitis C antibody, complete blood count, liver function tests, and lipid panel (for JAK inhibitors). Vaccination status must be reviewed, and live vaccines must be administered at least 2-4 weeks before therapy initiation.

## Drug Classes and Selection

### TNF Inhibitors

TNF inhibitors block tumor necrosis factor alpha. The context manages adalimumab (subcutaneous, biweekly), etanercept (subcutaneous, weekly), infliximab (intravenous infusion at weeks 0, 2, 6, then every 8 weeks), certolizumab pegol (subcutaneous), and golimumab (subcutaneous or intravenous). Each drug has specific dosing, route, frequency, and monitoring requirements modeled as InfusionProtocol or SubcutaneousProtocol value objects.

### IL-6 Receptor Blockers

IL-6 pathway inhibitors include tocilizumab (intravenous or subcutaneous) and sarilumab (subcutaneous). These agents require specific monitoring for neutropenia, liver enzyme elevation, and lipid changes. The SafetyMonitoring entity tracks lab monitoring at prescribed intervals (4-8 weeks after initiation, then every 3-6 months).

### JAK Inhibitors

Janus kinase inhibitors include tofacitinib, baricitinib, and upadacitinib. These oral agents are classified as targeted synthetic DMARDs. The context models their unique safety monitoring requirements including cardiovascular risk assessment, venous thromboembolism risk evaluation, malignancy screening, and complete blood count monitoring. FDA boxed warnings for cardiovascular events, malignancy, and thrombosis in patients over 50 with cardiovascular risk factors are enforced as domain rules.

### Other Biologics

Additional agents managed in this context include abatacept (T-cell co-stimulation modulator), rituximab (B-cell depleting agent requiring CD20 monitoring), and belimumab (BLyS inhibitor for SLE). Each has distinct protocols modeled within the aggregate.

## Infusion Management

For intravenous biologics, the InfusionSession entity documents each infusion visit. It records pre-medication administration (acetaminophen, diphenhydramine, and/or methylprednisolone as per protocol), vital signs at baseline and at defined intervals, infusion rate with any adjustments, the duration of post-infusion observation, and any infusion reactions with severity grading (mild, moderate, severe, life-threatening).

The InfusionProtocol value object defines standardized infusion parameters including initial rate, rate escalation schedule, and maximum rate. For infliximab, the standard protocol infuses over 2 hours with observation for at least 1 hour post-infusion, with the option for accelerated infusion (1 hour) after several well-tolerated standard infusions.

## Biosimilar Switching

The BiosimilarSwitchingService manages transitions between reference biologics and biosimilars. It evaluates patient eligibility (stable disease on current reference product), generates switching documentation, ensures patient notification and consent, defines a post-switch monitoring schedule, and tracks outcomes after the switch to detect any loss of efficacy or new adverse events.

The BiosimilarSwitch entity records the reference product, the biosimilar product, the switch date, clinical rationale, and post-switch assessment results. This documentation satisfies pharmacovigilance requirements and supports clinical decision-making about whether to continue the biosimilar or revert to the reference product.

## Domain Events

TherapyStarted is published when therapy begins after screening completion. InfusionReactionOccurred is published when an adverse infusion event is documented. BiosimilarSwitchCompleted is published when a product transition is executed. TherapyDiscontinued is published when therapy is stopped with the reason documented.

## Integration Points

This context consumes therapy requests from the Autoimmune Disease Context and the Joint Disease Context. It integrates with external pharmacy systems through an anti-corruption layer that translates between internal therapy models and NCPDP prescription formats. It publishes therapy events consumed by the Outcomes Tracking Context for longitudinal analysis.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Singh, J.A., et al. "2015 ACR Guideline for the Treatment of Rheumatoid Arthritis." Arthritis & Rheumatology, 2016.
- Smolen, J.S., et al. "EULAR Recommendations for RA Management." Annals of the Rheumatic Diseases, 2023.
- Kay, J., et al. "Consensus-Based Recommendations for Biosimilar Switching." Annals of the Rheumatic Diseases, 2018.
