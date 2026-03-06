# Oncology Domain - Ubiquitous Language

Aggregate Root: A cluster of domain objects treated as a single unit for data changes, such as a Treatment Plan or Chemotherapy Order.

Anti-Corruption Layer: A translation boundary that protects a bounded context from external system concepts, such as isolating the oncology domain from legacy laboratory information systems.

Biomarker: A measurable biological indicator used to assess cancer characteristics, guide treatment selection, or monitor therapeutic response.

Bounded Context: A logical boundary within which a specific domain model and its terminology are consistent, such as the Diagnosis & Staging Context or the Chemotherapy Management Context.

Brachytherapy: A form of radiation therapy where a sealed radiation source is placed inside or next to the area requiring treatment.

CTCAE Grade: A standardized adverse event severity rating from the Common Terminology Criteria for Adverse Events, used to monitor treatment toxicity.

Clinical Trial: A research study that tests new treatments or procedures in patients, with eligibility criteria, protocols, and outcome tracking.

Context Map: A visual representation of the relationships and interactions between bounded contexts in the oncology system.

Disease Stage: A classification of cancer extent using the TNM system, combining tumor size, node involvement, and metastasis status into an overall stage.

Domain Event: A significant occurrence within the oncology domain that triggers actions in other bounded contexts, such as DiagnosisConfirmed or ChemotherapyCycleCompleted.

Dose Calculation: The process of determining the appropriate drug quantity for a patient based on body surface area, renal function, and protocol specifications.

Fractionation Schedule: The plan dividing total radiation dose into individual treatment sessions delivered over a specified time period.

IGRT: Image-Guided Radiation Therapy, a technique using imaging during radiation treatment to improve precision and accuracy of dose delivery.

Infusion Schedule: The planned timing, sequence, and duration of chemotherapy drug administration during a treatment cycle.

Margin Assessment: The pathological evaluation of tissue borders after surgical resection to determine whether cancer cells are present at the edges.

Molecular Profile: The set of genetic mutations, gene expressions, and protein markers identified in a tumor that inform treatment decisions.

Multidisciplinary Tumor Board: A committee of oncology specialists who collaboratively review patient cases and recommend treatment strategies.

NCCN Guideline: A clinical practice guideline from the National Comprehensive Cancer Network that provides evidence-based treatment recommendations by cancer type and stage.

Pathology Report: A document describing the microscopic examination of tissue samples, including tumor type, grade, margins, and biomarker status.

Performance Status: A standardized measure of a patient's functional capacity, commonly assessed using the ECOG or Karnofsky scale, influencing treatment eligibility.

Regimen: A defined combination of chemotherapy drugs with specified doses, routes, schedules, and cycle lengths used to treat a specific cancer type.

Resection: The surgical removal of a tumor and surrounding tissue, classified by extent as wide local excision, segmental, or radical resection.

Sentinel Node Biopsy: A surgical procedure that identifies and examines the first lymph node to which cancer cells are most likely to spread.

Simulation: The planning session in radiation therapy where imaging is used to define treatment targets and patient positioning before dose delivery begins.

Survivorship Care Plan: A document summarizing cancer treatment received, recommended follow-up schedules, potential late effects, and wellness strategies.

TNM Classification: The international staging system that classifies cancer by Tumor size (T), lymph Node involvement (N), and distant Metastasis (M).

Treatment Plan: An aggregate root representing the comprehensive strategy for a patient's cancer care, including modality selection, sequencing, and goals.

Toxicity Monitoring: The systematic assessment of adverse effects during cancer treatment using standardized grading scales such as CTCAE.

Tumor Board Decision: The documented recommendation from a multidisciplinary tumor board regarding a patient's diagnosis, staging, and treatment approach.

Value Object: An immutable domain object defined by its attributes rather than identity, such as a TNM Classification or a Dose Calculation result.
