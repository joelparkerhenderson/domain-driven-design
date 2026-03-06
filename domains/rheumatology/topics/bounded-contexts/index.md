# Bounded Contexts in the Rheumatology Domain

## Overview

Bounded contexts are logical boundaries within which a particular domain model is defined and applicable. In the rheumatology domain, six bounded contexts encapsulate distinct areas of clinical practice, each with its own ubiquitous language, business rules, and invariants. These boundaries prevent model pollution and ensure that each context can evolve independently while maintaining well-defined integration points.

## Clinical Assessment Context

The Clinical Assessment Context manages the structured evaluation of patients presenting with musculoskeletal and autoimmune symptoms. Its primary responsibilities include documenting joint examinations using standardized 28-joint or 66/68-joint counts, ordering and interpreting serologic tests (rheumatoid factor, anti-CCP antibodies, ANA panels, complement levels), calculating composite disease activity indices (DAS28, CDAI, SDAI, RAPID3), and documenting physical examination findings.

Within this context, "assessment" refers specifically to a structured clinical evaluation performed at a point in time. The context owns the definition of what constitutes a complete examination and what scoring algorithms to apply. It does not make treatment decisions but produces the clinical data that other contexts consume.

Key invariants include: a DAS28 score requires all four components (TJC28, SJC28, ESR or CRP, patient global assessment); an ANA result must include both titer and pattern; and serology panels must be interpreted in the context of clinical findings, not in isolation.

## Autoimmune Disease Context

The Autoimmune Disease Context handles the classification, monitoring, and treatment strategy for systemic autoimmune diseases. It manages rheumatoid arthritis using ACR/EULAR 2010 classification criteria, systemic lupus erythematosus using SLICC/ACR criteria and SLEDAI scoring, systemic sclerosis with organ involvement tracking, ANCA-associated and large-vessel vasculitis, and Sjogren's disease with sicca symptom assessment.

Within this context, "disease activity" is defined specifically for each disease using validated instruments. Treatment strategies follow treat-to-target principles where therapy is escalated or adjusted until a predefined target is reached. The context manages DMARD selection, dose adjustments, and flare detection but delegates biologic prescribing details to the Biologic Therapy Context.

Key invariants include: classification criteria must be formally applied before a diagnosis is established; treatment escalation requires documented failure of prior therapy; and flare detection uses predefined thresholds specific to each disease.

## Joint Disease Context

The Joint Disease Context covers non-autoimmune joint diseases where the pathophysiology differs from systemic autoimmune conditions. It manages osteoarthritis staging and conservative treatment, gout diagnosis and urate-lowering therapy, calcium pyrophosphate deposition disease, and joint injection therapy including corticosteroid and hyaluronic acid injections.

Within this context, "joint disease" refers specifically to conditions where joint pathology is the primary concern rather than systemic autoimmunity. Synovial fluid analysis is a key diagnostic procedure, and crystal identification drives diagnosis. Treatment emphasizes local interventions and metabolic management rather than systemic immunomodulation.

Key invariants include: gout diagnosis requires crystal identification or validated clinical criteria; urate-lowering therapy targets a specific serum urate level; and joint injection procedures require documentation of indication, technique, and outcome.

## Biologic Therapy Context

The Biologic Therapy Context governs the specialized workflow for prescribing, administering, and monitoring biologic and targeted synthetic DMARDs. It manages TNF inhibitor selection and administration, IL-6 receptor blocker protocols, JAK inhibitor prescribing with safety monitoring, biosimilar switching protocols, and infusion center operations including pre-medication and monitoring.

Within this context, "therapy" refers specifically to biologic and targeted synthetic agents with their unique regulatory and safety requirements. The context enforces pre-treatment screening (tuberculosis, hepatitis B/C, infection risk), manages drug-specific monitoring schedules, and tracks adverse events for pharmacovigilance reporting.

Key invariants include: biologic therapy cannot begin without documented pre-treatment screening; biosimilar switches require patient notification and consent; and infusion reactions must be documented and graded according to standardized criteria.

## Pain Management Context

The Pain Management Context addresses the multimodal approach to pain that crosses disease boundaries. It manages pain assessment using validated scales (VAS, NRS, McGill Pain Questionnaire), physical therapy referrals and rehabilitation progress, occupational therapy for joint protection and adaptive equipment, pharmacologic pain management (NSAIDs, corticosteroids, analgesics), and patient education for self-management.

Within this context, "pain management" encompasses the full spectrum of interventions beyond disease-modifying therapy. The context coordinates with external therapy providers and tracks functional outcomes. It maintains its own model of patient functional status distinct from disease activity measures.

Key invariants include: pain management plans must address both pharmacologic and non-pharmacologic interventions; therapy referrals must include specific functional goals; and opioid prescribing must follow institutional and regulatory guidelines.

## Outcomes Tracking Context

The Outcomes Tracking Context provides longitudinal measurement and reporting of patient outcomes. It manages DAS28 trajectory tracking for treat-to-target monitoring, HAQ-DI functional disability assessment over time, radiographic progression scoring (Sharp/van der Heijde, Larsen), patient-reported outcomes (PROMIS, EQ-5D), and quality measure reporting for regulatory and payer requirements.

Within this context, "outcome" refers to a measured result at a specific time point that contributes to a longitudinal record. The context aggregates data from other contexts to provide a comprehensive view of disease trajectory and treatment effectiveness.

Key invariants include: outcome measures must use validated instruments with documented psychometric properties; radiographic scoring requires comparison with prior studies; and treat-to-target assessments must occur at defined intervals.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7.
- Aletaha, D., et al. "2010 Rheumatoid Arthritis Classification Criteria." Arthritis & Rheumatism, 2010.
- Smolen, J.S., et al. "EULAR Recommendations for the Management of Rheumatoid Arthritis." Annals of the Rheumatic Diseases, 2023.
