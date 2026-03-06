# Clinical Assessment Context in the Hormone Replacement Therapy Domain

The Clinical Assessment Context is a bounded context responsible for evaluating patients for hormone replacement therapy (HRT) candidacy. It encompasses symptom evaluation using validated clinical instruments, hormone panel interpretation with reference range analysis, medical history collection, contraindication screening, and the formal determination of whether a patient is an appropriate candidate for hormone therapy.

## Context Overview

This context represents the entry point of the HRT treatment lifecycle. Before any treatment protocol can be established, a comprehensive clinical assessment must determine the patient's symptom burden, endocrine status, risk profile, and eligibility. The Clinical Assessment Context owns this entire workflow, from initial patient intake through final candidacy determination, producing the assessment artifacts that downstream contexts consume.

## Domain Model

### Aggregates

The ClinicalAssessment aggregate is the primary aggregate root, encapsulating the complete assessment for a single patient evaluation episode. It maintains internal consistency by ensuring that candidacy status always reflects the current state of symptom scores, panel interpretations, and contraindication screen results. The PatientProfile aggregate maintains persistent patient baseline data that spans multiple assessment episodes.

### Key Entities

The ClinicalAssessment entity tracks the assessment lifecycle from initiation through completion. The SymptomEvaluation entity records the administration and scoring of a specific symptom instrument. The HormonePanelInterpretation entity captures the clinical interpretation of baseline hormone laboratory results.

### Key Value Objects

SymptomScore captures quantified instrument results. CandidacyStatus represents the eligibility determination. RiskFactorProfile aggregates clinical risk factors. HormoneLevel represents individual hormone measurements with reference range context.

## Clinical Workflows

### Symptom Evaluation

Patients complete validated symptom assessment instruments appropriate to their clinical indication. For menopausal symptoms, the Menopause Rating Scale (MRS) or Greene Climacteric Scale may be used. For male androgen deficiency, the ADAM (Androgen Deficiency in Aging Males) questionnaire or AMS (Aging Males' Symptoms) scale may be applied. The SymptomScoringService calculates composite scores and severity classifications from raw responses.

### Hormone Panel Interpretation

Baseline hormone panels typically include estradiol, progesterone, total and free testosterone, FSH, LH, DHEA-S, thyroid panel (TSH, free T3, free T4), and sex hormone-binding globulin (SHBG). The HormonePanelInterpretation entity captures not only individual values but also the clinical significance of hormone ratios and patterns. For example, elevated FSH with low estradiol confirms ovarian insufficiency, while low testosterone with elevated LH indicates primary hypogonadism.

### Contraindication Screening

The CandidacyEvaluationService screens against absolute contraindications including undiagnosed vaginal bleeding, active or recent breast cancer, active liver disease, active thromboembolic disease, and known hypersensitivity to hormone preparations. Relative contraindications are evaluated with risk-benefit analysis, including family history of breast cancer, history of thromboembolism, cardiovascular disease risk factors, and gallbladder disease.

### Candidacy Determination

The final candidacy determination integrates symptom severity, hormone panel findings, risk assessment, and patient preferences. The CandidacyEvaluationService produces a CandidacyStatus of approved (clear indication with acceptable risk), conditionally approved (indication present with manageable elevated risk requiring enhanced monitoring), deferred (requires additional evaluation or specialist consultation), or declined (contraindicated or risk exceeds benefit).

## Domain Events

The context publishes AssessmentCompleted when the evaluation reaches its final determination, CandidacyStatusChanged when reassessment alters the candidacy decision, and RiskFactorIdentified when new risk factors are discovered during evaluation.

## Context Relationships

The Clinical Assessment Context is upstream to the Hormone Protocol Context through a customer-supplier relationship. It shares a small kernel of risk factor data with the Side Effect Management Context. It maintains an anti-corruption layer when integrating with external electronic health record systems for patient demographics and medical history.

## Ubiquitous Language

Within this context, "assessment" always means a comprehensive HRT candidacy evaluation, not a general medical assessment. "Panel" refers specifically to the defined set of baseline hormone tests, not an arbitrary collection. "Candidacy" refers to HRT eligibility, not general treatment eligibility. "Screening" refers to contraindication evaluation, not general health screening.

## Clinical Standards

Assessment practices align with guidelines from the Endocrine Society, the North American Menopause Society, and the American Urological Association. Symptom instruments are used as validated, with scoring algorithms following published specifications. Contraindication lists reflect current clinical consensus and regulatory guidance.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- The Endocrine Society. "Testosterone Therapy in Men with Hypogonadism." The Journal of Clinical Endocrinology & Metabolism, 2018.
- North American Menopause Society. "The 2022 Hormone Therapy Position Statement." Menopause, 2022.
