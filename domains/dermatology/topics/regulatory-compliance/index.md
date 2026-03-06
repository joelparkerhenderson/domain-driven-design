# Regulatory Compliance in the Dermatology Domain

Regulatory compliance encompasses the legal and governance requirements that shape the dermatology domain model design. In dermatology systems, compliance requirements span patient privacy (HIPAA), medical device and drug regulation (FDA), clinical documentation standards, cosmetic procedure regulations, pathology laboratory accreditation, and state-level licensing and scope-of-practice rules. As Evans (2003) notes, regulatory constraints are cross-cutting concerns that influence domain model design across all bounded contexts.

## HIPAA Compliance

The Health Insurance Portability and Accountability Act governs the handling of protected health information (PHI) across all bounded contexts in the dermatology domain. HIPAA requirements influence domain model design in several ways.

The Privacy Rule requires that PHI is shared only with authorized recipients for permitted purposes. In the domain model, this means that domain events carrying patient information must include only the minimum necessary data for the consuming context to perform its function. The BiopsyPerformed event sent to the Pathology Integration Context carries clinical context but not the patient's full medical history.

The Security Rule requires safeguards for electronic PHI. While infrastructure-level security measures (encryption, access control) are outside the domain model, the model itself must support audit trails. Every domain event, state transition, and data access within the domain is auditable, with the acting user, timestamp, and action recorded. The domain model includes audit trail entities within each bounded context that capture clinically significant actions.

Clinical photography in dermatology presents specific HIPAA challenges because photographs may contain identifiable features. The Treatment Outcomes Context's Photo Comparison Series aggregate must enforce de-identification policies for any photographs shared beyond the treating provider's context.

## FDA Regulations

The Food and Drug Administration regulates medical devices and drugs that are integral to dermatology practice. FDA compliance influences the domain model in the following areas.

Injectable product tracking in the Cosmetic Services Context must satisfy FDA requirements for adverse event reporting. The domain model tracks product lot numbers, expiration dates, and patient associations so that in the event of a product recall or adverse event report, all affected patients can be identified. The InjectableTreatment entity carries FDA-relevant data (NDC code, lot number, manufacturer) that supports MedWatch adverse event reporting.

Laser devices used in the Procedure Management Context are FDA-regulated medical devices. The domain model tracks device identifiers, maintenance records, and calibration status. Procedure Session aggregates reference the specific device used, supporting compliance with FDA device tracking requirements.

Phototherapy equipment falls under FDA regulation as medical devices. The Phototherapy Plan aggregate tracks cumulative UV dose to comply with safety guidelines that limit long-term UV exposure, reflecting FDA-informed safety parameters.

## Clinical Documentation Standards

Dermatology documentation must meet standards established by the Centers for Medicare and Medicaid Services (CMS), the American Academy of Dermatology, and applicable state medical boards. These standards influence domain model design in multiple contexts.

The Clinical Assessment Context enforces documentation completeness rules. A Skin Examination aggregate cannot be finalized without documented findings for all examined areas, a clinical impression, and a plan of action. These are not merely data validation rules but domain invariants that reflect legal documentation requirements.

The Procedure Management Context enforces procedural documentation requirements. Procedure Session aggregates must include pre-procedure assessment, informed consent reference, procedure description with parameters used, immediate post-procedure assessment, and discharge instructions. For Mohs surgery specifically, documentation of each stage with tissue mapping and margin status is required for Medicare reimbursement.

The Pathology Integration Context enforces specimen requisition completeness. A Specimen Case cannot be submitted for processing without adequate clinical history, specimen site identification, and ordering provider information, as required by the College of American Pathologists (CAP) accreditation standards.

## Cosmetic Procedure Regulations

Cosmetic dermatology is subject to regulations that differ from medical dermatology and vary by jurisdiction. The Cosmetic Services Context models these regulatory requirements as domain invariants.

Informed consent for cosmetic procedures has specific requirements beyond standard medical consent. The Cosmetic Consent aggregate enforces disclosure of the elective nature of the procedure, expected outcomes with realistic expectations, specific risks including rare complications, available alternatives, product information (for injectables), and financial responsibility (since cosmetic procedures are typically not covered by insurance).

Some jurisdictions impose waiting periods between cosmetic consultation and procedure (cooling-off periods). The domain model enforces these waiting periods as invariants that prevent treatment administration before the required interval has elapsed.

Product-specific regulations govern the use of injectable neuromodulators and fillers. Off-label use (using FDA-approved products for non-approved indications) requires additional documentation and consent. The domain model tracks whether each injectable use is on-label or off-label and enforces enhanced consent requirements for off-label applications.

## Laboratory Accreditation

The Pathology Integration Context must account for laboratory accreditation requirements. Pathology laboratories that process dermatology specimens are typically accredited by the College of American Pathologists (CAP) and certified under the Clinical Laboratory Improvement Amendments (CLIA). These requirements influence the domain model's specimen tracking and chain of custody design.

CAP accreditation requires that specimen requisitions include specific clinical information, that chain of custody is maintained and documented, that turnaround times are tracked, and that critical results are communicated promptly. The Specimen Case aggregate's invariants reflect these accreditation requirements.

## State Licensing and Scope of Practice

State medical board regulations define who can perform specific dermatologic procedures and under what supervision. The domain model reflects scope-of-practice rules through provider role assignments within Procedure Session and Cosmetic Treatment Plan aggregates. The Procedure Management Context validates that the performing provider holds appropriate credentials for the procedure type. Certain procedures (such as Mohs surgery) may require fellowship-trained dermatologic surgeons, while others (such as cryotherapy for benign lesions) may be performed by physician assistants under supervision.

## Privacy in Telemedicine

As dermatology increasingly incorporates teledermatology, the domain model must accommodate privacy requirements specific to remote consultation. Clinical photographs transmitted for remote review, asynchronous store-and-forward consultations, and real-time video evaluations each have regulatory requirements for patient consent, data transmission security, and documentation. These requirements influence the Clinical Assessment Context's model for remote examination encounters.

## Compliance as Domain Knowledge

Regulatory compliance is treated as domain knowledge encoded in the model rather than as external rules imposed on the system. Vernon (2013) emphasizes that compliance rules that affect business behavior belong in the domain model. When a regulation changes, the domain model is updated to reflect the new business reality. Khononov (2021) adds that compliance requirements should be modeled as explicit domain concepts (such as consent entities, audit trail entries, and documentation completeness invariants) rather than as hidden infrastructure concerns.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. *HIPAA Privacy Rule and Security Rule*.
- U.S. Food and Drug Administration. *Medical Device Regulations*.
- Centers for Medicare and Medicaid Services. *Documentation Guidelines for Evaluation and Management Services*.
- College of American Pathologists. *Laboratory Accreditation Program Standards*.
- American Academy of Dermatology. *Position Statements on Practice Scope and Supervision*.
