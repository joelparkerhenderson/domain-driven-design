# Bounded Contexts in Oncology

## Overview

Bounded contexts define the logical boundaries within which a specific domain model and its terminology are consistent. In oncology, bounded contexts reflect the natural clinical and organizational divisions of cancer care. Each bounded context has its own ubiquitous language, its own aggregates and entities, and its own rules for maintaining consistency. Communication between bounded contexts occurs through well-defined interfaces, domain events, and translation layers.

## The Six Oncology Bounded Contexts

### Diagnosis and Staging Context

This context encompasses all activities related to identifying and classifying cancer. It includes cancer screening programs, diagnostic imaging interpretation, biopsy procedures, pathology analysis, TNM staging classification, and molecular profiling. The primary aggregate root is the Cancer Diagnosis, which encapsulates the histological type, grade, stage, and molecular markers.

Within this context, "stage" has a precise meaning defined by the AJCC TNM classification system. A pathology report is the authoritative document that establishes the diagnosis. Molecular profiling results, such as HER2 status or EGFR mutations, are value objects attached to the diagnosis that influence treatment selection.

### Treatment Planning Context

This context manages the collaborative process of formulating a comprehensive treatment strategy. It includes tumor board case presentation and review, multidisciplinary treatment plan creation, clinical trial eligibility screening, and alignment with NCCN guideline recommendations. The primary aggregate root is the Treatment Plan, which captures the selected modalities, sequencing, and treatment intent (curative or palliative).

Within this context, a "treatment plan" represents the consensus recommendation of the multidisciplinary team, not an individual physician's order. The tumor board decision is a domain event that triggers downstream treatment ordering in the modality contexts.

### Chemotherapy Management Context

This context handles the ordering, preparation, and administration of systemic cancer therapies. It includes regimen selection from standardized protocols, dose calculation based on patient parameters (body surface area, renal function, performance status), infusion scheduling, pre-medication protocols, and toxicity monitoring using CTCAE grading. The primary aggregate root is the Chemotherapy Order, which contains the regimen, calculated doses, and administration schedule.

Within this context, "dose" always refers to the calculated amount for a specific patient, not the protocol-defined standard dose. A "cycle" represents one complete iteration of the regimen's drug administration pattern.

### Radiation Therapy Context

This context manages the planning and delivery of therapeutic radiation. It includes simulation sessions for treatment setup, radiation treatment planning with dose optimization, fractionation schedule design, image-guided radiation therapy (IGRT), and brachytherapy procedures. The primary aggregate root is the Radiation Treatment Plan, which defines the target volumes, dose constraints, and delivery technique.

Within this context, "plan" refers specifically to the radiation dosimetric plan, distinct from the broader treatment plan in the Treatment Planning Context. "Fractions" are the individual treatment sessions that together deliver the prescribed total dose.

### Surgical Oncology Context

This context addresses the planning and execution of surgical cancer interventions. It includes preoperative assessment and resection planning, intraoperative decision-making, margin assessment from surgical pathology, sentinel lymph node biopsy, and reconstructive planning. The primary aggregate root is the Surgical Case, which captures the planned procedure, findings, and outcomes.

Within this context, "margin" refers to the distance between the edge of the resected tissue and the nearest cancer cells. A "negative margin" (also called "clear margin") indicates no cancer cells at the tissue edge. The margin assessment directly influences whether additional treatment is needed.

### Survivorship Care Context

This context manages the long-term care of cancer patients after completing active treatment. It includes follow-up surveillance protocols, monitoring for late and long-term treatment effects, psychosocial support coordination, wellness and lifestyle recommendations, and survivorship care plan generation. The primary aggregate root is the Survivorship Care Plan, which documents the treatment summary and recommended follow-up schedule.

Within this context, "survivorship" begins at the point of diagnosis but this context primarily focuses on the post-treatment phase. "Late effects" are treatment-related complications that emerge months or years after treatment completion.

## Boundary Enforcement

Each bounded context maintains strict ownership of its domain model. The Diagnosis and Staging Context owns the definition of what constitutes a confirmed diagnosis. The Chemotherapy Management Context owns the rules for dose calculation and modification. No external context can directly modify another context's aggregates.

When information must flow between contexts, it passes through defined integration points. A staging result from the Diagnosis and Staging Context is translated into a treatment-relevant summary when consumed by the Treatment Planning Context. This translation preserves the autonomy of each context's model while enabling collaborative workflows.

## Context Ownership and Teams

Each bounded context should be owned by a team with the relevant domain expertise. The Diagnosis and Staging Context is best maintained by a team that includes pathologists and diagnostic radiologists. The Chemotherapy Management Context requires input from medical oncologists and oncology pharmacists. This alignment between context ownership and clinical expertise ensures that the domain model remains accurate and clinically meaningful.

## Evolution of Context Boundaries

Bounded context boundaries in oncology may evolve as clinical practice changes. The emergence of immunotherapy and targeted therapy has blurred traditional distinctions between chemotherapy and other systemic therapies. Precision medicine initiatives may require closer integration between the Diagnosis and Staging Context and the Treatment Planning Context. Strategic design must accommodate this evolution through flexible integration patterns.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6: Bounded Contexts.
