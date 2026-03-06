# Regulatory Compliance: Autism Domain

Regulatory compliance encompasses the legal and governance requirements that shape domain model design, business rules, and data handling practices across all bounded contexts in the autism spectrum management domain.

## Purpose

Autism management systems operate under multiple overlapping regulatory frameworks spanning healthcare, education, disability rights, and privacy. These regulations impose constraints on how data is collected, stored, shared, and used; how services are authorized, delivered, and documented; and how individual rights are protected. The domain model must encode these regulatory requirements as business rules and invariants to ensure that the system inherently enforces compliance rather than relying on external checks.

## Healthcare Privacy Regulations

### HIPAA (Health Insurance Portability and Accountability Act)

HIPAA governs the privacy and security of protected health information (PHI) in the autism domain. Key impacts on the domain model:

- Minimum necessary standard: The domain model restricts data shared between bounded contexts to the minimum necessary for the receiving context's purpose. Published language interfaces expose only the data elements required by consuming contexts, not complete aggregate states.
- Individual access rights: Individuals and their legal representatives have the right to access their records. The domain model supports record export across all bounded contexts in accessible formats.
- Authorization requirements: Sharing PHI with external parties (schools, insurance companies, other providers) requires documented authorization. The domain model tracks authorization status and scope.
- Breach notification: The domain model includes audit logging sufficient to identify the scope of any data breach and support notification obligations.
- Business associate agreements: External system integrations must be governed by business associate agreements, tracked within the integration layer.

### HIPAA and Minors

Since many individuals receiving autism services are minors, the domain model must handle the intersection of HIPAA with state laws governing parental access to minor children's health information. In most states, parents or legal guardians serve as personal representatives for minors and exercise HIPAA rights on their behalf.

## Educational Privacy Regulations

### FERPA (Family Educational Rights and Privacy Act)

FERPA governs the privacy of education records, directly impacting the Educational Planning Context:

- Education record definition: IEPs, progress reports, behavioral data collected at school, and transition plans are education records protected under FERPA.
- Parent/student rights: Parents (and eligible students age 18+) have the right to inspect and review education records and to request corrections.
- Disclosure restrictions: Education records may not be disclosed without consent except under specific exceptions (e.g., school officials with legitimate educational interest, health or safety emergencies).
- Directory information: The domain model distinguishes between directory information and protected education records to apply appropriate access controls.

### HIPAA-FERPA Intersection

When healthcare providers deliver services in educational settings (e.g., school-based ABA therapy), the domain model must correctly identify whether records are governed by HIPAA (maintained by the healthcare provider) or FERPA (maintained by the educational agency). Records maintained by a school for educational purposes are governed by FERPA, not HIPAA, even if they contain health information.

## Disability Rights Regulations

### IDEA (Individuals with Disabilities Education Act)

IDEA imposes specific procedural requirements on the Educational Planning Context:

- Free Appropriate Public Education (FAPE): The domain model enforces that every eligible student has a current IEP that provides FAPE in the least restrictive environment.
- IEP procedural requirements: The IEP Document Aggregate enforces all content requirements, team composition rules, timeline mandates (annual review, triennial reevaluation), and parent participation rights.
- Procedural safeguards: The domain model tracks prior written notice requirements, consent for evaluation and placement, and dispute resolution processes (mediation, due process hearings).
- Transition mandates: The Transition Plan Aggregate enforces the requirement that transition planning begin no later than the IEP in effect when the student turns 16 (or earlier per state law).
- Child find: The domain model supports the obligation to identify, locate, and evaluate children with suspected disabilities.

### ADA (Americans with Disabilities Act)

The ADA prohibits discrimination based on disability and requires reasonable accommodations. In the autism domain, ADA requirements inform:

- Accommodation documentation: The domain model tracks reasonable accommodation requests, determinations, and implementation across educational, employment, and community settings.
- Communication access: AAC device and service provision may be required as a reasonable accommodation under the ADA.
- Transition planning: Post-secondary employment and community living goals must consider ADA protections and accommodation rights.

## Insurance and Service Authorization Regulations

### State Autism Insurance Mandates

Most states have enacted autism insurance mandates requiring health insurers to cover autism-related services, including diagnostic assessment, ABA therapy, speech therapy, and occupational therapy. These mandates vary by state and impact the domain model through:

- Coverage requirements: The domain model tracks applicable state mandate provisions, including covered services, age limits, annual dollar or hour caps, and provider qualification requirements.
- Authorization processes: The Intervention Episode entity manages service authorization timelines, reauthorization requirements, and authorization status.
- Medical necessity documentation: The Outcomes Tracking Context generates treatment effectiveness data supporting medical necessity determinations for authorization and reauthorization.

### Medicaid Requirements

For individuals receiving Medicaid-funded services, additional regulatory requirements apply:

- Early and Periodic Screening, Diagnostic, and Treatment (EPSDT): For Medicaid-eligible children under 21, EPSDT requires coverage of all medically necessary services, which may exceed state plan limitations.
- Home and Community-Based Services (HCBS) waivers: The domain model tracks waiver-specific service definitions, provider qualifications, and documentation requirements.

## Informed Consent

Informed consent requirements span multiple regulatory frameworks and affect all bounded contexts:

- Assessment consent: The Diagnostic Assessment Context requires documented consent before administering standardized instruments.
- Treatment consent: The Behavioral Intervention Context requires informed consent for behavior intervention plans, including explanation of procedures, risks, benefits, and alternatives.
- Service consent: The Educational Planning Context requires consent for initial evaluation and initial placement in special education.
- Data sharing consent: Cross-context data sharing and external system integration require appropriate consent or authorization documentation.

The domain model tracks consent status, scope, expiration, and revocation across all contexts. Consent withdrawal triggers business rules that restrict data sharing and service delivery accordingly.

## Mandatory Reporting

Healthcare and educational professionals are mandatory reporters for suspected child abuse and neglect. The domain model includes:

- Reporting protocols: Documented procedures for identifying and reporting suspected abuse or neglect.
- Report tracking: Records of reports made, including date, nature of concern, and reporting agency.
- Confidentiality protections: Mandatory reports are handled with appropriate confidentiality protections within the domain model.

## Audit and Compliance Monitoring

The domain model supports regulatory compliance through:

- Audit trails: All significant state changes across bounded contexts are logged with timestamps, user identity, and change details.
- Compliance dashboards: The Outcomes Tracking Context aggregates compliance metrics (IEP review timeliness, authorization status, consent currency) for monitoring.
- Regulatory change management: The domain model accommodates regulatory changes through value object versioning and configurable business rules.

## References

- U.S. Department of Health and Human Services. (1996). *Health Insurance Portability and Accountability Act*. 45 CFR Parts 160, 162, 164.
- U.S. Department of Education. (1974). *Family Educational Rights and Privacy Act*. 20 U.S.C. 1232g; 34 CFR Part 99.
- Individuals with Disabilities Education Improvement Act. (2004). 20 U.S.C. 1400 et seq.
- Americans with Disabilities Act. (1990). 42 U.S.C. 12101 et seq.
- National Conference of State Legislatures. (2023). *Autism and Insurance Coverage: State Laws*.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
