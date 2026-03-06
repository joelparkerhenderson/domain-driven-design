# Medical DDD - Ubiquitous Language

Adjudication: The process by which an insurance payer evaluates a submitted claim and determines the payment amount, denials, and patient responsibility.
Adverse Drug Event (ADE): An injury or harm resulting from the use of a medication, including allergic reactions, side effects, and overdoses.
Aggregate: A cluster of domain objects treated as a single unit for data consistency, such as PatientRecord with ProblemList and MedicalHistory.
Aggregate Root: The single entity through which all access to an aggregate is managed, ensuring invariant enforcement.
Anti-Corruption Layer: A translation boundary that protects a bounded context's domain model from external system concepts and data formats.
Assessment: A clinical evaluation of a patient's condition, symptoms, and findings used to formulate a diagnosis or treatment plan.
Authorization (Prior): A requirement from an insurance payer that a provider obtain approval before delivering specific services or medications.
Bounded Context: A well-defined boundary within which a particular domain model applies and terms have specific meaning.
C-CDA: Consolidated Clinical Document Architecture, an HL7 standard for exchanging structured clinical documents between systems.
Care Plan: A documented set of clinical goals, interventions, and timelines for managing a patient's treatment across encounters.
Chief Complaint: The primary reason a patient seeks medical attention, recorded in the patient's own words at the start of an encounter.
Claim: A formal request submitted by a provider to an insurance payer for reimbursement of medical services rendered.
Clinical Decision Support (CDS): Automated rules and alerts that assist clinicians in making evidence-based decisions at the point of care.
Clinical Pathway: A standardized, evidence-based protocol defining the expected course of treatment for a specific diagnosis or procedure.
Context Map: A visual and conceptual representation of the relationships and interactions between bounded contexts.
Contraindication: A condition or factor that makes a particular treatment, medication, or procedure inadvisable for a patient.
Copay: A fixed amount a patient pays out-of-pocket for a covered medical service or prescription at the time of service.
Core Domain: The most critical and complex subdomain that provides competitive advantage and requires the most investment.
CPT Code: Current Procedural Terminology code used to describe medical, surgical, and diagnostic services for billing and reimbursement.
DEA Number: A unique identifier assigned by the Drug Enforcement Administration to a prescriber authorized to prescribe controlled substances.
Deductible: The amount a patient must pay out-of-pocket for covered services before the insurance plan begins to pay.
DICOM: Digital Imaging and Communications in Medicine, the international standard for transmitting, storing, and sharing medical images.
Diagnosis: A clinical determination of a patient's disease or condition based on examination, testing, and medical history.
Domain Event: A record of something significant that happened in the domain, such as PrescriptionIssued or ClaimSubmitted.
Domain Service: An operation that belongs to the domain but does not naturally fit within a single entity or value object.
Dosage: An immutable value representing the prescribed amount, frequency, route, and duration of a medication.
Drug Interaction: A clinically significant effect that occurs when two or more medications are taken together, potentially causing harm.
Electronic Health Record (EHR): A comprehensive digital record of a patient's health information shared across multiple healthcare organizations.
Encounter: A single interaction between a patient and healthcare provider, whether in-person, virtual, inpatient, or outpatient.
Entity: A domain object defined by its unique identity that persists over time, such as Patient, Encounter, or Prescription.
Episode of Care: The complete sequence of healthcare services provided to a patient for a specific condition over time.
Explanation of Benefits (EOB): A document from an insurance payer detailing what was covered, what was paid, and what the patient owes.
FHIR: Fast Healthcare Interoperability Resources, an HL7 standard for exchanging healthcare information electronically using RESTful APIs.
Formulary: A list of medications approved and covered by an insurance plan, often organized by tier to reflect patient cost-sharing.
Generic Subdomain: A subdomain that is not specific to the organization and can use off-the-shelf solutions, such as email or authentication.
HIPAA: Health Insurance Portability and Accountability Act, the U.S. federal law governing healthcare data privacy and security.
HITECH: Health Information Technology for Economic and Clinical Health Act, strengthening HIPAA enforcement and promoting EHR adoption.
HL7: Health Level Seven International standards for the exchange, integration, and retrieval of electronic health information.
ICD-10: International Classification of Diseases, 10th revision, used worldwide for coding diagnoses and health conditions.
Imaging Study: A diagnostic examination using radiological modalities such as X-ray, CT, MRI, or ultrasound to visualize internal structures.
Integration Pattern: A design approach for managing data exchange and communication between bounded contexts.
Lab Order: A request for laboratory testing on patient specimens, including test type, priority, and clinical indication.
Lab Result: The outcome of a laboratory test, including measured values, reference ranges, abnormal flags, and interpretation.
LOINC: Logical Observation Identifiers Names and Codes, a universal standard for identifying medical laboratory observations and clinical measurements.
Medication Administration Record (MAR): A document tracking when and how medications are given to a patient, including dose, time, and route.
Medical History: A comprehensive record of a patient's past illnesses, surgeries, medications, and family health patterns.
Medical Record Number (MRN): A unique identifier assigned to a patient within a healthcare organization.
NDC: National Drug Code, a unique identifier for human drugs in the United States that identifies labeler, product, and package.
NPI: National Provider Identifier, a unique 10-digit identification number assigned to healthcare providers in the United States.
Order: A clinical directive from a provider for a specific action such as a lab test, imaging study, medication, or referral.
Pathology Report: A detailed analysis of tissue or cell samples by a pathologist, used to diagnose diseases such as cancer.
Patient: The central entity representing an individual receiving or registered to receive healthcare services.
Pharmacy Benefit Manager (PBM): A third-party organization that administers prescription drug programs on behalf of insurance plans.
Prescription: A provider's written directive authorizing the dispensing of a specific medication with dosage and administration instructions.
Problem List: A curated, maintained list of a patient's active and resolved medical conditions, diagnoses, and concerns.
Provider: A licensed healthcare professional authorized to diagnose, treat, and prescribe for patients.
Published Language: A well-documented shared data format used for communication between bounded contexts.
Referral: A formal request from one provider to another for specialized evaluation or treatment of a patient.
Remote Patient Monitoring (RPM): The use of connected devices to collect and transmit patient health data from home to a clinical team.
Repository: A mechanism for encapsulating storage, retrieval, and search behavior for aggregate roots.
RxNorm: A standardized nomenclature for clinical drugs maintained by the NLM, providing normalized names for medications.
Shared Kernel: A small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together.
SNOMED CT: Systematized Nomenclature of Medicine Clinical Terms, a comprehensive clinical terminology for electronic health records.
Subdomains: Classification of domain areas by strategic importance as core, supporting, or generic.
Supporting Subdomain: A subdomain that is necessary for the business but not a core differentiator, such as scheduling or notifications.
Telehealth: The broad use of electronic information and telecommunications technologies to support long-distance clinical care and education.
Telemedicine: The practice of providing clinical care remotely through video, audio, or messaging technology between provider and patient.
Value Object: An immutable domain object defined by its attributes rather than identity, such as ICD10Code, CPTCode, or Dosage.
Virtual Visit: A real-time audio-video encounter between a patient and provider conducted through a telemedicine platform.
Vital Signs: A set of clinical measurements (temperature, blood pressure, heart rate, respiratory rate, oxygen saturation) indicating physiological status.
