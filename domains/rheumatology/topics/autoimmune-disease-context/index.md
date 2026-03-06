# Autoimmune Disease Context

## Overview

The Autoimmune Disease Context manages the classification, monitoring, and treatment strategy for systemic autoimmune diseases encountered in rheumatology practice. This context handles rheumatoid arthritis (RA), systemic lupus erythematosus (SLE), systemic sclerosis (scleroderma), vasculitis syndromes, and Sjogren's disease. It applies evidence-based classification criteria, implements treat-to-target strategies, detects disease flares, and guides treatment escalation decisions. This is a core subdomain where the most sophisticated domain modeling is required.

## Context Boundaries

The Autoimmune Disease Context owns disease classification logic, disease activity monitoring, flare detection, and treatment strategy formulation. It does not perform clinical assessments (owned by the Clinical Assessment Context), manage biologic prescribing details (owned by the Biologic Therapy Context), or track long-term outcomes (owned by the Outcomes Tracking Context). It consumes assessment data and produces treatment strategy decisions.

## Aggregate: DiseaseManagementPlan

The DiseaseManagementPlan aggregate root represents the ongoing management of a single autoimmune disease for a single patient. A patient with both RA and SLE would have two separate DiseaseManagementPlan aggregates, each with its own classification, activity monitoring, and treatment strategy.

The aggregate contains the AutoimmuneDisease entity (diagnosis with classification criteria), TreatmentGoal value objects (defining the treat-to-target objective), MedicationRegimen entities (current and historical therapy), and FlareEpisode entities (documenting flare events). The aggregate enforces that classification criteria must be formally met before treatment can begin and that treatment escalation requires documented inadequate response.

## Rheumatoid Arthritis Management

RA classification uses the 2010 ACR/EULAR criteria. The DiseaseClassificationService scores four domains: joint involvement (1 large joint scores 0, 2-10 large joints scores 1, 1-3 small joints scores 2, 4-10 small joints scores 3, more than 10 joints with at least 1 small joint scores 5), serology (negative RF and anti-CCP scores 0, low-positive scores 2, high-positive scores 3), acute phase reactants (normal ESR and CRP scores 0, abnormal scores 1), and duration (less than 6 weeks scores 0, 6 weeks or greater scores 1). A total score of 6 or greater classifies the patient as having RA.

RA treatment follows a treat-to-target strategy. The treatment target is remission (DAS28 below 2.6 or CDAI at or below 2.8) or, when remission is not feasible, low disease activity. The TreatToTargetService evaluates whether the target is achieved at each assessment and recommends therapy adjustment when it is not. First-line therapy is methotrexate (a csDMARD), with escalation to combination csDMARDs or bDMARDs/tsDMARDs for inadequate responders.

## Systemic Lupus Erythematosus Management

SLE management within this context tracks disease activity using the SLEDAI (SLE Disease Activity Index), which scores 24 clinical and laboratory descriptors across nine organ systems. The DiseaseActivityLevel value object captures the SLEDAI score with interpretation (no activity equals 0, mild activity 1-5, moderate activity 6-10, high activity 11-19, very high activity 20 or above).

SLE classification uses the 2019 EULAR/ACR criteria with an ANA entry criterion followed by additive scoring of clinical domains (constitutional, hematologic, neuropsychiatric, mucocutaneous, serosal, musculoskeletal, renal) and immunologic domains (anti-dsDNA, complement, antiphospholipid antibodies). A score of 10 or greater classifies SLE.

Treatment strategies for SLE vary by organ involvement and severity, with hydroxychloroquine as background therapy for all patients.

## Scleroderma, Vasculitis, and Sjogren's Disease

Systemic sclerosis management tracks skin involvement using the modified Rodnan skin score and monitors for organ complications (interstitial lung disease, pulmonary arterial hypertension, renal crisis). The context classifies limited versus diffuse disease and tracks autoantibody profiles (anti-centromere, anti-Scl-70, anti-RNA polymerase III).

Vasculitis management classifies by vessel size (large-vessel including giant cell arteritis and Takayasu, medium-vessel including polyarteritis nodosa, and small-vessel including ANCA-associated vasculitis). Disease activity is tracked using BVAS (Birmingham Vasculitis Activity Score).

Sjogren's disease management tracks sicca symptoms, salivary gland function, and systemic manifestations. The context applies ACR/EULAR classification criteria and monitors for lymphoma risk.

## Domain Events

DiseaseFlareDetected is published when disease activity rises above threshold. RemissionAchieved is published when sustained remission is confirmed. TreatmentEscalated is published when therapy is advanced due to inadequate response. These events drive downstream actions in other bounded contexts.

## Integration Points

This context consumes AssessmentCompleted events from the Clinical Assessment Context. It publishes treatment strategy decisions that the Biologic Therapy Context uses to initiate or adjust therapy. The Outcomes Tracking Context aggregates disease activity trajectories produced by this context.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Aletaha, D., et al. "2010 Rheumatoid Arthritis Classification Criteria." Arthritis & Rheumatism, 2010.
- Aringer, M., et al. "2019 EULAR/ACR Classification Criteria for SLE." Annals of the Rheumatic Diseases, 2019.
- Smolen, J.S., et al. "EULAR Recommendations for RA Management: 2022 Update." Annals of the Rheumatic Diseases, 2023.
