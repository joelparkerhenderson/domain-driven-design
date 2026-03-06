# Joint Disease Context

## Overview

The Joint Disease Context manages non-autoimmune joint diseases where the primary pathology is mechanical, metabolic, or crystal-driven rather than systemic autoimmune. This context covers osteoarthritis (OA), gout, calcium pyrophosphate deposition disease (CPPD or pseudogout), and joint injection therapy. These conditions are common in rheumatology practice and require distinct management approaches from autoimmune diseases. The context handles disease staging, crystal identification, urate management, and procedural documentation for intra-articular injections.

## Context Boundaries

The Joint Disease Context owns the classification, staging, and management of non-autoimmune joint conditions. It manages synovial fluid analysis interpretation, urate-lowering therapy for gout, osteoarthritis staging, and joint injection procedures. It does not manage autoimmune arthritis (owned by the Autoimmune Disease Context), biologic agent prescribing (owned by the Biologic Therapy Context), or pain management strategies beyond disease-specific interventions (owned by the Pain Management Context).

The boundary reflects a fundamental clinical distinction: autoimmune joint diseases involve systemic immune dysregulation requiring immunomodulatory therapy, while the diseases in this context involve local joint pathology requiring targeted mechanical, metabolic, or procedural interventions.

## Aggregate: JointDiseasePlan

The JointDiseasePlan aggregate root represents the management strategy for a specific joint condition in a specific patient. It contains the JointDisease entity (with classification and staging), SynovialFluidAnalysis value objects (crystal identification and cell count), InjectionRecord entities (documenting procedures), and for gout, UrateLoweringTherapy entities (medication, dose, target urate level, monitoring schedule).

The aggregate enforces invariants such as: gout diagnosis requires crystal identification or validated clinical criteria (2015 ACR/EULAR gout classification criteria); urate-lowering therapy must target serum urate below 6 mg/dL (or below 5 mg/dL for tophaceous gout); and joint injection intervals must respect minimum spacing guidelines.

## Osteoarthritis Management

Osteoarthritis in this context is classified using the Kellgren-Lawrence radiographic grading system: grade 0 (no features), grade 1 (doubtful), grade 2 (minimal with definite osteophytes), grade 3 (moderate with joint space narrowing), and grade 4 (severe with bone-on-bone contact). The RadiographicGrade value object captures this assessment.

OA management focuses on conservative treatment including weight management, exercise prescription, acetaminophen, NSAIDs, and intra-articular injections. The context tracks which joints are affected, their current grade, symptom severity, and treatment history. Surgical referral criteria (persistent pain despite optimal conservative management in grade 3-4 disease) are modeled as domain rules.

The context distinguishes primary OA (age-related degeneration) from secondary OA (post-traumatic, inflammatory, or metabolic). Secondary OA may have an overlap with the Autoimmune Disease Context when it results from prior inflammatory arthritis.

## Gout and Crystal Arthropathy

Gout management is a key workflow in this context. The CrystalAnalysis value object captures synovial fluid findings: monosodium urate crystals appear as negatively birefringent needle-shaped crystals under compensated polarized light microscopy. Calcium pyrophosphate crystals appear as weakly positively birefringent rhomboid crystals. The SynovialFluidAnalysisService interprets these findings in combination with cell count and Gram stain to establish diagnosis.

The 2015 ACR/EULAR gout classification criteria are applied by the DiseaseClassificationService within this context. The criteria use a step-wise approach: entry criterion (at least one episode of peripheral joint swelling, pain, or tenderness), sufficient criterion (MSU crystals in symptomatic joint), and if crystals are not identified, a scoring system across clinical, laboratory, and imaging domains.

Urate-lowering therapy management follows the 2020 ACR guideline for gout. The UrateTargetService evaluates current serum urate against target and recommends dose adjustments. Allopurinol dosing starts low (100 mg daily, 50 mg in chronic kidney disease) and titrates upward every 2-4 weeks until the serum urate target is achieved.

## Joint Injection Therapy

Joint injection documentation is managed through the JointInjection entity. Each injection records the target joint (knee, shoulder, wrist, small hand joints, etc.), the approach used (anterior, lateral, posterior), the medications injected (corticosteroid type such as triamcinolone or methylprednisolone with dose, and/or hyaluronic acid product), the volume injected, any aspiration performed, and the patient's immediate response.

The context tracks injection history per joint to enforce clinical guidelines on injection frequency. Generally, corticosteroid injections to a single joint should not exceed 3-4 per year. The aggregate enforces this rule by checking prior injection dates.

## Domain Events

InjectionAdministered is published when a joint injection procedure is completed. GoutFlareOccurred is published when a gout flare episode is documented. UrateTargetAchieved is published when serum urate reaches the treatment target after urate-lowering therapy initiation.

## Integration Points

This context consumes assessment data from the Clinical Assessment Context, particularly joint examination findings and laboratory results (serum urate, inflammatory markers). It publishes events consumed by the Pain Management Context (for injection-related pain management) and the Outcomes Tracking Context (for longitudinal joint disease tracking).

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- FitzGerald, J.D., et al. "2020 ACR Guideline for Management of Gout." Arthritis Care & Research, 2020.
- Neogi, T., et al. "2015 ACR/EULAR Gout Classification Criteria." Annals of the Rheumatic Diseases, 2015.
- Kolasinski, S.L., et al. "2019 ACR/AF Guideline for Management of Osteoarthritis." Arthritis Care & Research, 2020.
