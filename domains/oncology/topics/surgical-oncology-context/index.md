# Surgical Oncology Context

## Overview

The Surgical Oncology Context addresses the planning, execution, and assessment of surgical interventions for cancer treatment. Surgery is often the primary treatment modality for solid tumors, and this bounded context manages the complex workflow from preoperative planning through intraoperative decision-making to postoperative pathological assessment. It includes resection planning, margin assessment, sentinel lymph node biopsy, lymph node dissection, and reconstructive planning. Surgical outcomes directly influence subsequent treatment decisions across other bounded contexts.

## Ubiquitous Language

Within this context, a "resection" is the surgical removal of a tumor and surrounding tissue. "Margins" refer to the distance between the edge of the resected tissue and the nearest cancer cells. A "negative margin" (or "clear margin") means no cancer cells are found at the tissue edge. A "positive margin" means cancer cells are present at the resection boundary, often necessitating additional treatment. A "sentinel node" is the first lymph node to which cancer cells from a primary tumor are most likely to spread.

## Aggregate Roots

### Surgical Case

The Surgical Case is the primary aggregate root. It encompasses the preoperative plan (planned procedure type, surgical approach, anticipated extent of resection), the operative record (actual procedure performed, intraoperative findings, estimated blood loss, complications), the specimen tracking (specimens submitted for pathological examination), the margin assessment results, and the sentinel node biopsy results.

The aggregate enforces several invariants. A surgical case cannot be marked as completed without documented margin assessment results for all resected specimens. If intraoperative findings necessitate a change from the planned procedure, the deviation must be documented with clinical justification. The sentinel node biopsy result must be recorded before a decision on full lymph node dissection can be finalized.

### Reconstructive Plan

The Reconstructive Plan aggregate manages planning for reconstructive surgery following oncologic resection. It contains the type of reconstruction (immediate or delayed), the reconstructive technique selected, the tissue source (autologous flap, implant, combination), and the planned staging of reconstruction if multi-stage. The aggregate enforces that reconstruction timing is compatible with planned adjuvant therapies (for example, immediate breast reconstruction must account for potential post-mastectomy radiation).

## Key Entities

**Operative Procedure**: An entity representing a single surgical intervention. It captures the procedure type (wide local excision, segmental resection, radical resection, lymph node dissection), the surgical approach (open, laparoscopic, robotic), the anatomic site, the start and end times, the operating surgeon and assistants, intraoperative findings, and complications.

**Surgical Specimen**: An entity tracking a tissue specimen from excision through pathological evaluation. Each specimen has a unique identifier, an anatomic origin, a laterality designation, and orientation markers placed by the surgeon. The specimen links to the Pathology Case aggregate in the Diagnosis and Staging Context for histological examination.

**Sentinel Node**: An entity representing an identified sentinel lymph node. Records the identification method (blue dye, radiotracer, combination), the anatomic station, and the pathological result (positive or negative for metastatic cancer cells, including isolated tumor cells and micrometastases).

**Intraoperative Consultation**: An entity representing a real-time pathology assessment performed during surgery, such as a frozen section analysis. Records the clinical question posed, the specimen examined, the preliminary result communicated to the surgeon, and the impact on intraoperative decision-making.

## Key Value Objects

**Surgical Margin**: The pathological assessment of a resection border, including the margin status (positive, negative, close), the closest margin distance in millimeters, the margin location, and the tissue type at the margin.

**Lymph Node Status**: The aggregate assessment of lymph node involvement from a dissection specimen, including the total nodes examined, nodes positive for metastasis, presence of extranodal extension, and the largest metastatic deposit size.

**Resection Extent**: A classification of the surgical removal scope (R0 for complete resection with negative margins, R1 for microscopic residual disease at margins, R2 for macroscopic residual disease).

**Operative Complexity**: A categorization of the surgical procedure's complexity level based on anatomic factors, extent of resection, and vascular or neural involvement.

## Domain Events

**SurgicalCasePlanned**: A surgical procedure has been planned based on the approved treatment plan.
**SurgicalProcedureStarted**: The operative procedure has commenced.
**IntraoperativeFindingRecorded**: A significant intraoperative finding has been documented that may alter the surgical approach.
**SurgicalProcedureCompleted**: The operative procedure has been completed.
**SpecimenSubmitted**: A surgical specimen has been submitted for pathological examination.
**MarginAssessmentReported**: The pathological margin assessment is available.
**SentinelNodeResultReported**: The sentinel lymph node pathology result is available.
**PositiveMarginIdentified**: A positive surgical margin has been found, potentially triggering re-excision or adjuvant therapy.
**ReconstructionCompleted**: A reconstructive procedure has been performed.

## Domain Services

**ResectionAdequacyAssessmentService**: Evaluates whether the surgical resection achieved adequate margins based on margin assessment results, cancer type, and established adequacy criteria for the specific tumor site.

**ReconstructionEligibilityService**: Assesses a patient's eligibility for reconstructive surgery considering resection extent, overall health, planned adjuvant therapies, and patient preferences.

## Integration Points

This context is downstream of the Treatment Planning Context, receiving surgical intervention directives. The anti-corruption layer translates the treatment plan's surgical modality selection into the surgical context's preoperative planning workflow.

This context publishes surgical pathology results (margin assessments, lymph node status) that flow to the Diagnosis and Staging Context for pathological staging updates. A positive margin or unexpected lymph node involvement may trigger restaging, which in turn may trigger treatment plan revision in the Treatment Planning Context. This creates a feedback loop that reflects clinical reality: surgical findings often change the clinical picture and subsequent treatment approach.

The Surgical Oncology Context also interfaces with the Survivorship Care Context by publishing treatment completion events that document the surgical procedures performed for inclusion in the survivorship care plan.

## Clinical Workflow

The surgical workflow within this context follows a defined sequence. Preoperative planning establishes the intended procedure based on the treatment plan directive. Simulation sessions for complex cases may define the surgical approach in detail. Intraoperative decision-making adapts the plan based on findings during surgery. Postoperative specimen submission triggers the pathological evaluation workflow. Margin and node results return to complete the surgical case record and may trigger additional treatment planning.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American College of Surgeons. Commission on Cancer Standards. https://www.facs.org/quality-programs/cancer-programs
- Society of Surgical Oncology. SSO Clinical Practice Guidelines. https://www.surgonc.org
