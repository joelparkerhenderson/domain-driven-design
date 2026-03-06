# Treatment Planning Context

## Overview

The Treatment Planning Context manages the collaborative process of formulating a comprehensive cancer treatment strategy. It orchestrates the multidisciplinary decision-making that is central to modern oncology, where specialists from different disciplines evaluate each patient's case and collectively determine the optimal approach. This context includes tumor board case presentation and review, treatment plan creation, clinical trial eligibility assessment, and alignment with NCCN guideline recommendations.

## Ubiquitous Language

Within this context, a "treatment plan" is the comprehensive, multidisciplinary-approved strategy for managing a patient's cancer, not a single physician's order. "Treatment intent" distinguishes between curative (aiming for cancer elimination) and palliative (aiming for symptom control and quality of life) goals. A "tumor board decision" is the consensus recommendation from the multidisciplinary team. "Guideline concordance" measures whether the treatment plan aligns with NCCN guideline recommendations for the patient's cancer type and stage.

## Aggregate Roots

### Treatment Plan

The Treatment Plan is the primary aggregate root. It captures the treatment intent (curative or palliative), the selected treatment modalities (surgery, chemotherapy, radiation, immunotherapy, targeted therapy), the planned sequencing of modalities (neoadjuvant, concurrent, adjuvant), the tumor board decision that approved the plan, and references to the guiding NCCN pathway.

The aggregate enforces several invariants. A treatment plan must have an approved tumor board decision before it can be activated. The modality sequence must be clinically valid: neoadjuvant therapy must precede surgery in the timeline, adjuvant therapy must follow surgery. The treatment intent must be consistent with the selected modalities and the patient's disease stage.

### Clinical Trial Enrollment

The Clinical Trial Enrollment aggregate tracks a patient's participation in a specific clinical trial. It contains the trial protocol reference, the eligibility assessment with the criteria that were evaluated, the randomization arm assignment (if applicable), the enrollment date, and protocol compliance tracking. The aggregate enforces that enrollment cannot proceed without documented eligibility confirmation.

## Key Entities

**Tumor Board Case**: An entity representing a patient case presented to the multidisciplinary tumor board. Each case has a unique identifier, a presentation date, the presenting physician, attending specialists with their disciplines, the clinical summary presented, and the documented recommendation. A patient may be presented to the tumor board multiple times as their clinical situation evolves.

**Treatment Modality Selection**: An entity representing the selection of a specific treatment modality within the treatment plan. Each selection specifies the modality type, the intended role (definitive, neoadjuvant, adjuvant, concurrent), the responsible specialist, and the planned timing relative to other modalities.

**Guideline Reference**: An entity linking the treatment plan to a specific NCCN guideline pathway and version. Tracks which guideline recommendations were considered and whether the plan is guideline-concordant or represents an evidence-based deviation.

## Key Value Objects

**Treatment Intent**: Either curative or palliative, fundamentally shaping the treatment approach.

**Modality Sequence**: The ordered arrangement of treatment modalities, capturing temporal relationships (before, concurrent with, after).

**Eligibility Criteria Assessment**: The evaluation of a specific trial eligibility criterion, including the criterion description, the patient's value, and the assessment result (met, not met, pending).

**NCCN Pathway Reference**: A reference to a specific NCCN guideline, version, cancer type, and pathway within the guideline.

## Domain Events

**TumorBoardCasePresented**: A patient case has been formally presented to the tumor board.
**TumorBoardRecommendationIssued**: The tumor board has reached a consensus recommendation.
**TreatmentPlanApproved**: The treatment plan has been finalized and approved for execution.
**TreatmentPlanRevised**: The treatment plan has been modified based on new clinical information or treatment response.
**ClinicalTrialEligibilityDetermined**: A patient's eligibility for a specific trial has been assessed.
**ClinicalTrialEnrollmentCompleted**: A patient has been formally enrolled in a clinical trial.

## Domain Services

**ClinicalTrialMatchingService**: Evaluates a patient's clinical profile against available trial eligibility criteria to identify potentially eligible trials.

**GuidelineRecommendationService**: Maps a patient's diagnosis, stage, and molecular profile to NCCN guideline-recommended treatment options.

**TreatmentSequencingService**: Determines the optimal sequencing of treatment modalities based on cancer type, stage, and evidence-based sequencing rules.

## Integration Points

This context is downstream of the Diagnosis and Staging Context, consuming diagnosis confirmation and staging events that trigger treatment planning workflows. It is upstream to the three treatment modality contexts (Chemotherapy Management, Radiation Therapy, Surgical Oncology), publishing approved treatment plan directives that each modality context translates into specific clinical orders.

The relationship with modality contexts follows the customer-supplier pattern. The Treatment Planning Context specifies what treatment should be delivered (modality, intent, sequencing). Each modality context determines how to deliver that treatment within its own domain expertise. If a modality context determines that the planned treatment cannot be safely delivered, it raises an event that triggers treatment plan reconsideration.

## Clinical Standards

NCCN Clinical Practice Guidelines provide the primary evidence-based framework for treatment recommendations. ASCO guidelines supplement NCCN for specific clinical scenarios. The National Cancer Institute (NCI) clinical trial registry provides the catalog of available trials for eligibility matching. Institutional tumor board policies govern case presentation and recommendation documentation requirements.

## Consistency Boundaries

The Treatment Plan aggregate maintains strong consistency between its modality selections, their sequencing, and the tumor board decision. Changes to the modality sequence or the addition of new modalities require validation against the treatment intent and clinical sequencing rules. Cross-context consistency with the modality contexts is eventual, mediated by domain events.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- National Comprehensive Cancer Network. NCCN Clinical Practice Guidelines. https://www.nccn.org/guidelines
- American Society of Clinical Oncology. ASCO Guidelines. https://www.asco.org/guidelines
