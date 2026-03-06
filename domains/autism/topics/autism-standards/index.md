# Autism Standards: Autism Domain

Autism-specific clinical, professional, and regulatory standards inform the design of domain models, value objects, published languages, and business rules across all bounded contexts in the autism spectrum management domain.

## Purpose

Standards play a critical role in domain-driven design by providing the authoritative definitions, classifications, and protocols that shape the ubiquitous language and constrain the domain model. In the autism domain, multiple standards bodies define diagnostic criteria, professional practice guidelines, and service delivery requirements. These standards must be accurately reflected in the domain model to ensure clinical validity, regulatory compliance, and interoperability with external systems.

## Diagnostic Standards

### DSM-5 (Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition)

The DSM-5, published by the American Psychiatric Association (2013), defines the diagnostic criteria for autism spectrum disorder. It directly informs the domain model through:

- Diagnostic code structure: The DSM5DiagnosticCode value object encodes the 299.00 diagnostic code and its specifiers.
- Two-domain criteria model: The diagnostic evaluation model requires assessment of both social communication/interaction deficits (Criterion A) and restricted/repetitive behaviors (Criterion B).
- Severity level classification: The SeverityLevel value object implements the three-level support needs framework (Level 1: Requiring support, Level 2: Requiring substantial support, Level 3: Requiring very substantial support) applied separately to each diagnostic domain.
- Specifiers: The diagnostic model captures specifiers for intellectual impairment, language impairment, known genetic/medical conditions, and co-occurring neurodevelopmental/mental/behavioral disorders.

### DSM-5-TR (Text Revision, 2022)

The DSM-5-TR updated the descriptive text for autism spectrum disorder while maintaining the same diagnostic criteria. The domain model tracks which edition (DSM-5 or DSM-5-TR) was applied during diagnosis, as this may be relevant for historical records and insurance documentation.

### ICD-11 (International Classification of Diseases, Eleventh Revision)

The ICD-11, published by the World Health Organization (2019), provides the international diagnostic classification system. ICD-11 code 6A02 (Autism spectrum disorder) aligns with the DSM-5 but uses a different classification structure. The domain model supports mapping between DSM-5 and ICD-11 codes for international reporting and insurance billing purposes.

## Assessment Instrument Standards

### ADOS-2 (Autism Diagnostic Observation Schedule, Second Edition)

The ADOS-2 (Lord et al., 2012) defines standardized administration and scoring protocols that constrain the InstrumentAdministration aggregate. The domain model enforces:

- Module selection rules based on age and expressive language level.
- Item-level scoring constraints (0, 1, 2, 3 or 0, 1, 2 depending on the item).
- Algorithm domain score computation rules for Social Affect and Restricted and Repetitive Behavior.
- Comparison score calculation (1-10 scale) based on module, age, and total score.
- Classification cutoffs (non-spectrum, autism spectrum, autism) specific to each module.

### ADI-R (Autism Diagnostic Interview, Revised)

The ADI-R (Rutter, Le Couteur, & Lord, 2003) defines structured interview protocols and algorithm scoring rules. The domain model enforces algorithm cutoffs for each diagnostic domain and tracks both current behavior and most abnormal behavior codes.

### M-CHAT-R/F (Modified Checklist for Autism in Toddlers, Revised with Follow-Up)

The M-CHAT-R/F (Robins, Fein, & Barton, 2009) defines scoring algorithms and risk classification thresholds that constrain the screening process model. The two-stage screening protocol (initial questionnaire followed by structured follow-up) is enforced by the screening process workflow.

## Professional Practice Standards

### BACB (Behavior Analyst Certification Board) Standards

BACB standards directly inform the Behavioral Intervention Context:

- Ethics Code for Behavior Analysts (2022): Defines professional conduct requirements including informed consent, data-based decision making, supervisory responsibilities, and client welfare obligations. These ethics requirements are modeled as business rules within the Behavioral Intervention Context.
- Certification and supervision standards: Define the qualifications required for BCBAs, BCaBAs, and RBTs, informing the Therapist entity's credential tracking and the supervision model's requirements.
- Task list: Defines the competency areas for behavior analysts, informing the scope of skill acquisition programs and assessment procedures.

### ASHA (American Speech-Language-Hearing Association) Practice Guidelines

ASHA guidelines inform the Communication Support Context:

- Roles and Responsibilities of Speech-Language Pathologists in Diagnosis, Assessment, and Treatment of Autism Spectrum Disorders Across the Lifespan (2006): Defines the scope of SLP services for individuals with autism.
- AAC practice guidelines: Inform the AAC feature matching process, device trial protocols, and communication assessment standards.

### AOTA (American Occupational Therapy Association) Practice Guidelines

AOTA guidelines inform the Sensory Processing Context:

- Practice guidelines for occupational therapy with individuals with autism: Define the scope of OT services including sensory processing assessment, sensory integration intervention, and environmental modification.
- Sensory Integration Fidelity Measure: Defines fidelity criteria for Ayres Sensory Integration intervention that constrain sensory therapy session documentation.

## Educational Standards

### IDEA (Individuals with Disabilities Education Act)

IDEA is the federal law governing special education services. It directly informs the Educational Planning Context through:

- IEP content requirements: Define the mandatory components of an IEP document, enforced by the IEP Document Aggregate.
- Procedural safeguards: Define parent rights, consent requirements, and dispute resolution processes.
- Transition planning requirements: Define when transition planning must begin and what components must be addressed.
- Least restrictive environment mandate: Constrains placement determination decisions.

### Section 504 and ADA

Section 504 of the Rehabilitation Act and the Americans with Disabilities Act inform accommodation requirements across educational and community settings.

## Data and Interoperability Standards

### HL7 FHIR (Fast Healthcare Interoperability Resources)

HL7 FHIR provides healthcare data interoperability standards that inform the published language interfaces between the autism domain and external healthcare systems. The anti-corruption layers at external integration points map between FHIR resource formats and internal domain model representations.

### CPT and HCPCS Codes

Current Procedural Terminology (CPT) and Healthcare Common Procedure Coding System (HCPCS) codes standardize billing for autism-related services (ABA therapy, speech therapy, occupational therapy, diagnostic assessments). These codes inform the external reporting integration layer.

## Standards Governance

Standards evolve over time. The domain model must accommodate standards version tracking, supporting transitions between versions (e.g., DSM-IV-TR to DSM-5) and maintaining historical records under the version in effect at the time of assessment or diagnosis. Value objects that encode standards-defined classifications include the standard version as an attribute.

## References

- American Psychiatric Association. (2013). *DSM-5*. APA Publishing.
- American Psychiatric Association. (2022). *DSM-5-TR*. APA Publishing.
- World Health Organization. (2019). *ICD-11*. WHO.
- Behavior Analyst Certification Board. (2022). *Ethics Code for Behavior Analysts*. BACB.
- American Speech-Language-Hearing Association. (2006). *Roles and Responsibilities of SLPs in ASD*. ASHA.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
