# Gerontology Standards in the Gerontology Domain

## Overview

Gerontology and geriatric care are governed by numerous clinical standards, assessment instruments, and practice guidelines that directly influence domain model design. In Domain-Driven Design, standards inform the structure of value objects, the behavior of domain services, and the vocabulary of the ubiquitous language. Understanding and correctly modeling these standards is essential for building a gerontology system that clinicians trust and regulators accept.

Evans (2003) notes that domain-specific standards often serve as published languages within a bounded context, providing universally understood formats for expressing domain concepts. In gerontology, standardized assessment instruments and clinical guidelines fulfill this role, defining how clinical data is captured, scored, interpreted, and communicated.

## Assessment Instrument Standards

### Katz Index of Independence in Activities of Daily Living

Published by Katz et al. (1963), this instrument assesses six basic ADL functions: bathing, dressing, toileting, transferring, continence, and feeding. Each function is scored as independent (1) or dependent (0), producing a total score from 0 to 6. The Katz Index defines the ADLScore value object in the Functional Independence Context. The instrument's structure determines the value object's attributes and validation rules.

### Lawton Instrumental Activities of Daily Living Scale

Published by Lawton and Brody (1969), this instrument assesses eight IADL functions: telephone use, shopping, food preparation, housekeeping, laundry, transportation, medication management, and finances. Scoring varies by function but produces an overall score from 0 to 8. The Lawton Scale defines the IADLScore value object and provides the published language for communicating functional independence data across contexts.

### Mini-Mental State Examination (MMSE)

Published by Folstein, Folstein, and McHugh (1975), the MMSE is a 30-point screening test assessing orientation, registration, attention and calculation, recall, and language. Scores of 24-30 indicate normal cognition, 18-23 indicate mild impairment, 10-17 indicate moderate impairment, and 0-9 indicate severe impairment. The MMSE defines one variant of the CognitiveTestScore value object in the Cognitive Health Context.

### Montreal Cognitive Assessment (MoCA)

Published by Nasreddine et al. (2005), the MoCA is a 30-point screening instrument designed to detect mild cognitive impairment with higher sensitivity than the MMSE. It assesses visuospatial/executive function, naming, memory, attention, language, abstraction, delayed recall, and orientation. The MoCA defines another variant of the CognitiveTestScore value object and is increasingly preferred over the MMSE for detecting early cognitive decline.

### Clinical Frailty Scale (CFS)

Developed by Rockwood et al. (2005), the CFS is a 9-point clinical judgment scale ranging from 1 (very fit) to 9 (terminally ill). It provides a quick, clinician-rated frailty assessment that defines the FrailtyScore value object when the CFS instrument is used. The scale's ordinal nature influences how frailty trajectory analysis is modeled.

### Fried Frailty Phenotype

Published by Fried et al. (2001), this criteria-based approach defines frailty as the presence of three or more of five criteria: unintentional weight loss, self-reported exhaustion, weakness (low grip strength), slow walking speed, and low physical activity. The phenotype defines an alternative FrailtyScore value object structure and influences the FrailtyScreening aggregate's invariant rules.

## Medication Standards

### AGS Beers Criteria

Published and periodically updated by the American Geriatrics Society, the Beers Criteria is an evidence-based list of potentially inappropriate medications for older adults. The criteria categorize medications into groups: avoid in older adults, avoid in certain conditions, use with caution, and drug-drug interactions to avoid. The Beers Criteria define the MedicationAppropriatenessClassification value object and drive the BeersScreeningService in the Medication Management Context.

### STOPP/START Criteria

Published by O'Mahony et al. (2015), STOPP (Screening Tool of Older Persons' Prescriptions) identifies potentially inappropriate medications, while START (Screening Tool to Alert to Right Treatment) identifies potential prescribing omissions. These criteria complement the Beers list and may be modeled as an alternative screening service within the Medication Management Context.

## Care Planning Standards

### Patient-Centered Medical Home (PCMH)

The PCMH model defines standards for coordinated, comprehensive primary care that are directly relevant to the Care Coordination Context. PCMH principles of team-based care, care coordination, and whole-person orientation inform the aggregate design and workflow modeling.

### Transitional Care Model (TCM)

Developed by Naylor et al. (2011), the TCM provides evidence-based protocols for managing care transitions. The model's components (discharge planning, follow-up, medication reconciliation, patient education) inform the CareTransition aggregate's structure and invariants.

## End-of-Life Standards

### POLST Paradigm

The Physician Orders for Life-Sustaining Treatment paradigm provides a standardized approach to translating patient treatment preferences into portable medical orders. POLST forms are recognized in most states and define the structure of the POLSTOrder entity in the End of Life Planning Context.

### National Consensus Project for Quality Palliative Care

The Clinical Practice Guidelines for Quality Palliative Care define eight domains of palliative care (structure and processes, physical aspects, psychological aspects, social aspects, spiritual aspects, cultural aspects, care of the dying, and ethical/legal aspects) that inform the PalliativeCarePlan aggregate structure.

## Standards as Published Language

In the gerontology domain, clinical standards serve as natural published languages. When the Cognitive Health Context publishes a MoCA score of 22, any clinician or system familiar with the MoCA standard can interpret that score without additional context. Similarly, when the Functional Independence Context publishes a Katz ADL score of 3, the clinical meaning is universally understood. This standardization reduces the need for custom integration formats between bounded contexts.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Katz, S. et al. (1963). Studies of Illness in the Aged: The Index of ADL. JAMA.
- Folstein, M.F. et al. (1975). Mini-Mental State. Journal of Psychiatric Research.
- Nasreddine, Z.S. et al. (2005). The Montreal Cognitive Assessment, MoCA. JAGS.
- Fried, L.P. et al. (2001). Frailty in Older Adults. The Journals of Gerontology.
- American Geriatrics Society. (2023). AGS Beers Criteria. JAGS.
