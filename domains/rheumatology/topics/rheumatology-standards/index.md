# Rheumatology Standards

## Overview

Domain-specific standards inform the design of value objects, domain services, and published languages within the rheumatology domain model. Rheumatology is governed by well-established clinical standards from organizations including the American College of Rheumatology (ACR), the European Alliance of Associations for Rheumatology (EULAR), and Outcome Measures in Rheumatology (OMERACT). These standards define classification criteria, treatment guidelines, outcome measurement instruments, and quality metrics that must be faithfully represented in the domain model.

## American College of Rheumatology (ACR)

### Classification Criteria

ACR has developed or co-developed classification criteria for most rheumatic diseases. These criteria are modeled as ClassificationCriteria value objects in the domain. The 2010 ACR/EULAR RA classification criteria use a point-based scoring system across four domains with a threshold of 6 points for classification. The 1997 ACR SLE criteria require 4 of 11 clinical and laboratory features. The 1990 ACR vasculitis classification criteria define specific criteria for granulomatosis with polyangiitis, polyarteritis nodosa, and other vasculitis subtypes.

These classification criteria are implemented in the DiseaseClassificationService, which scores each domain according to the published algorithm and determines whether the classification threshold is met. The service must be updated when criteria are revised, as occurred with the 2019 EULAR/ACR SLE criteria and the 2022 ACR/EULAR criteria for ANCA-associated vasculitis.

### Treatment Guidelines

ACR publishes treatment guidelines that inform domain rules in the Autoimmune Disease Context and Joint Disease Context. The 2015 ACR RA treatment guideline recommends methotrexate monotherapy as first-line DMARD therapy, escalation to combination DMARDs or biologic/targeted synthetic DMARDs for inadequate responders, and treat-to-target monitoring at regular intervals.

The 2020 ACR gout management guideline recommends urate-lowering therapy for patients with two or more flares per year, tophaceous gout, or radiographic damage. It recommends allopurinol as first-line urate-lowering therapy with gradual dose titration. These guidelines are modeled as domain rules within the relevant aggregates.

### Quality Measures

ACR participates in defining quality measures that the Outcomes Tracking Context must support. The Rheumatology Informatics System for Effectiveness (RISE) registry collects quality measure data including disease activity assessment frequency, functional status documentation, tuberculosis screening before biologic therapy, and glucocorticoid management.

## European Alliance of Associations for Rheumatology (EULAR)

### Treatment Recommendations

EULAR publishes evidence-based treatment recommendations that serve as the primary treatment algorithms in many rheumatology practices. The EULAR RA management recommendations (2022 update) define a treatment algorithm with phases: initial csDMARD therapy (phase I), escalation to bDMARD or tsDMARD after inadequate response (phase II), and switching within or between drug classes (phase III).

These phased recommendations are modeled in the DiseaseManagementPlan aggregate's treatment escalation logic. The TreatToTargetService evaluates treatment response against EULAR-defined response criteria (good response, moderate response, no response based on DAS28 change and final DAS28 level).

EULAR SLE management recommendations define treatment strategies stratified by organ involvement and severity. Lupus nephritis, neuropsychiatric lupus, and hematologic lupus each have distinct treatment algorithms modeled as domain rules.

### Response Criteria

EULAR response criteria define what constitutes a meaningful response to therapy. For RA, the EULAR response matrix combines the DAS28 change from baseline with the final DAS28 level to classify response as good, moderate, or none. These criteria are implemented in the Autoimmune Disease Context to determine when treatment escalation is warranted.

## Outcome Measures in Rheumatology (OMERACT)

### Core Outcome Sets

OMERACT defines core outcome sets that specify which outcomes must be measured in clinical trials and clinical practice for each rheumatic disease. For RA, the core set includes tender and swollen joint counts, patient and physician global assessments, pain, physical function (HAQ), acute phase reactant (ESR or CRP), and radiographic joint damage.

These core outcome sets inform the design of the OutcomeReport aggregate in the Outcomes Tracking Context. Each element of the core set corresponds to a value object or entity within the aggregate. The context ensures that all core outcomes are collected at appropriate intervals.

### OMERACT Filter

The OMERACT Filter 2.0 evaluates outcome measures against three criteria: truth (does the measure capture what is intended?), discrimination (does the measure detect meaningful differences?), and feasibility (can the measure be applied in practice?). Instruments used in the domain model must have passed the OMERACT Filter, ensuring that the outcome data collected is scientifically valid and clinically meaningful.

## Drug Nomenclature Standards

Biologic and targeted synthetic DMARDs follow WHO International Nonproprietary Names (INN). The INN system provides standardized naming with stems that indicate drug class: "-cept" for receptor fusion proteins (etanercept), "-mab" for monoclonal antibodies (adalimumab), "-nib" for kinase inhibitors (tofacitinib). Biosimilar naming follows FDA guidelines with distinguishing suffixes. These naming conventions are encoded in the domain model's drug identification value objects.

## Laboratory Standards

Serologic testing follows standardized assay methods and reporting formats. RF measurement uses nephelometry or ELISA with results reported in IU/mL. Anti-CCP uses ELISA with manufacturer-specific units. ANA uses indirect immunofluorescence on HEp-2 cells with titer and pattern reporting. These standards inform the SerologyResult value object design.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Aletaha, D., et al. "2010 Rheumatoid Arthritis Classification Criteria." Arthritis & Rheumatism, 2010.
- Smolen, J.S., et al. "EULAR Recommendations for the Management of RA: 2022 Update." Annals of the Rheumatic Diseases, 2023.
- Boers, M., et al. "Developing Core Outcome Sets: OMERACT Filter 2.0." Journal of Clinical Epidemiology, 2014.
- Singh, J.A., et al. "2015 ACR Guideline for the Treatment of RA." Arthritis & Rheumatology, 2016.
- FitzGerald, J.D., et al. "2020 ACR Guideline for Management of Gout." Arthritis Care & Research, 2020.
