# Hospital Management DDD - Ubiquitous Language

Admission: The formal process of registering a patient for inpatient care within the hospital.
Aggregate: A cluster of domain objects treated as a single unit for data consistency, such as Patient with MedicalRecord and Allergies.
Aggregate Root: The single entity through which all access to an aggregate is managed, ensuring invariant enforcement.
Allergy: A recorded adverse reaction a patient has to a specific substance, medication, or environmental factor.
Anti-Corruption Layer: A translation boundary that protects a bounded context's domain model from external system concepts.
Appointment: A scheduled time slot linking a patient to a healthcare provider for a specific clinical purpose.
Attending Physician: The doctor with primary responsibility for a patient's care during a hospital stay.
Audit Trail: A chronological record of system activities that provides documentary evidence of actions performed.
Bed Assignment: The allocation of a specific hospital bed to an admitted patient based on care needs and availability.
Billing Code: A standardized code (CPT, ICD-10) used to identify medical procedures and diagnoses for reimbursement.
Blood Type: An immutable classification of a patient's blood (A, B, AB, O with Rh factor) critical for transfusions.
Bounded Context: A well-defined boundary within which a particular domain model applies and terms have specific meaning.
Care Plan: A documented set of clinical goals, interventions, and timelines for managing a patient's treatment.
Chief Complaint: The primary reason a patient seeks medical attention, recorded in their own words.
Clinical Pathway: A standardized, evidence-based treatment protocol for a specific diagnosis or procedure.
Consent: A patient's documented authorization for a specific medical procedure, treatment, or data sharing.
Context Map: A visual and conceptual representation of the relationships and interactions between bounded contexts.
Core Domain: The most critical and complex subdomain that provides competitive advantage and requires the most investment.
CPT Code: Current Procedural Terminology code used to describe medical, surgical, and diagnostic services for billing.
Department: An organizational unit within the hospital specializing in a particular area of medicine or administration.
Diagnosis: A clinical determination of a patient's disease or condition based on examination, testing, and medical history.
Discharge: The formal process of releasing a patient from hospital care with instructions and follow-up plans.
Domain Event: A record of something significant that happened in the domain, such as PatientAdmitted or AppointmentScheduled.
Domain Service: An operation that belongs to the domain but does not naturally fit within a single entity or value object.
Dosage: An immutable value representing the prescribed amount, frequency, and route of a medication.
Electronic Medical Record (EMR): A digital version of a patient's medical history maintained by a single healthcare organization.
Emergency Department (ED): The hospital unit providing immediate care for acute illnesses and injuries.
Encounter: A single interaction between a patient and healthcare provider, whether inpatient, outpatient, or emergency.
Entity: A domain object defined by its unique identity that persists over time, such as Patient, Doctor, or Appointment.
Episode of Care: The complete sequence of healthcare services provided to a patient for a specific condition over time.
FHIR: Fast Healthcare Interoperability Resources, a standard for exchanging healthcare information electronically.
Generic Subdomain: A subdomain that is not specific to the organization and can use off-the-shelf solutions, such as email or authentication.
HL7: Health Level Seven International standards for the exchange, integration, and retrieval of electronic health information.
HIPAA: Health Insurance Portability and Accountability Act, the U.S. federal law governing healthcare data privacy and security.
ICD-10: International Classification of Diseases, 10th revision, used worldwide for coding diagnoses and health conditions.
Insurance Claim: A formal request submitted to an insurance company for payment of medical services rendered.
Integration Pattern: A design approach for managing data exchange and communication between bounded contexts.
Inpatient: A patient who has been formally admitted to the hospital for at least one overnight stay.
Lab Order: A request for laboratory testing on patient specimens, including test type, priority, and clinical indication.
Lab Result: The outcome of a laboratory test, including measured values, reference ranges, and interpretation.
LOINC: Logical Observation Identifiers Names and Codes, a universal standard for identifying medical laboratory observations.
Medical History: A comprehensive record of a patient's past illnesses, surgeries, medications, and family health patterns.
Medical Record Number (MRN): A unique identifier assigned to a patient within a healthcare organization.
Nurse Assignment: The allocation of a nursing professional to provide care for specific patients during a shift.
Outpatient: A patient who receives medical treatment without being admitted for an overnight hospital stay.
Patient: The central entity representing an individual receiving or registered to receive healthcare services.
Pharmacy Order: A prescription request for medication, including drug name, dosage, route, frequency, and duration.
Provider: A licensed healthcare professional authorized to diagnose, treat, and prescribe for patients.
Published Language: A well-documented shared data format used for communication between bounded contexts.
Referral: A formal request from one provider to another for specialized evaluation or treatment of a patient.
Repository: A mechanism for encapsulating storage, retrieval, and search behavior for aggregate roots.
Room Allocation: The assignment of a physical space within the hospital for patient care or procedures.
Shared Kernel: A small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together.
SNOMED CT: Systematized Nomenclature of Medicine Clinical Terms, a comprehensive clinical terminology for electronic health records.
Supporting Subdomain: A subdomain that is necessary for the business but not a core differentiator, such as scheduling or notifications.
Transfer: The movement of a patient from one department, unit, or facility to another during an episode of care.
Treatment Plan: A detailed clinical strategy for addressing a patient's diagnosed conditions, including therapies and medications.
Triage: The process of assessing and prioritizing patients based on the severity and urgency of their medical conditions.
Value Object: An immutable domain object defined by its attributes rather than identity, such as Address, BloodType, or Dosage.
Vital Signs: A set of clinical measurements (temperature, blood pressure, heart rate, respiratory rate) indicating physiological status.
Ward: A hospital division or section designated for patients with similar medical needs or conditions.
