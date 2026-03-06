# Regulatory Compliance: Attention-Deficit Domain

Regulatory compliance addresses the legal and governance requirements that shape the design of the ADHD management domain model, including healthcare privacy, educational records protection, disability rights, controlled substance regulations, and informed consent requirements.

## Purpose

ADHD management operates at the intersection of healthcare, education, and pharmacology, each governed by distinct regulatory frameworks. The domain model must enforce compliance with these regulations as intrinsic domain rules, not as afterthought infrastructure concerns. Data access controls, consent management, record retention, and disclosure rules are domain logic that shapes aggregate design, event publishing, and cross-context integration patterns.

## Healthcare Privacy: HIPAA

The Health Insurance Portability and Accountability Act governs the handling of protected health information (PHI) across the clinical bounded contexts.

### Impact on Domain Model Design

- **Minimum Necessary Standard**: Each bounded context should only access the minimum PHI necessary for its specific function. The Educational Accommodation Context does not need the full clinical assessment record; it needs only the eligibility-relevant findings translated through the anti-corruption layer.
- **Access Control as Domain Logic**: Role-based access to patient assessment data, treatment plans, medication records, and outcome measurements is a domain concern. The domain model defines which roles (clinician, therapist, prescriber, educator, researcher) can access which aggregate data.
- **Audit Trail Requirements**: All access to and modification of PHI must be logged. Domain events serve double duty as both integration mechanisms and audit trail entries. The event store provides a complete history of who accessed or modified clinical data and when.
- **De-identification for Research**: The Outcomes Tracking Context must support de-identification of patient data for population-level analysis and research, following HIPAA Safe Harbor or Expert Determination methods.
- **Breach Notification**: The domain model must support identification of affected records and patients in the event of a data breach, enabling timely notification as required by the HIPAA Breach Notification Rule.

### PHI Elements in the ADHD Domain

PHI elements that require HIPAA protection include: patient identifiers, diagnostic codes, assessment dates, treatment records, medication history, behavioral monitoring data, provider identifiers, and insurance information. Every aggregate root containing these elements must enforce HIPAA-compliant access controls.

## Educational Records: FERPA

The Family Educational Rights and Privacy Act governs the handling of student educational records, directly impacting the Educational Accommodation Context.

### Impact on Domain Model Design

- **Parent Access Rights**: Parents (or eligible students aged 18+) have the right to inspect and review all educational records, including IEP and 504 plan documentation. The Accommodation Plan aggregate must support parent access requests.
- **Consent for Disclosure**: Educational records generally cannot be disclosed without written parent consent. When clinical assessment data is translated through the anti-corruption layer into the Educational Accommodation Context, the resulting educational record becomes subject to FERPA protections.
- **Directory Information Exception**: Basic student information (name, grade, school) may be disclosed without consent if designated as directory information. The domain model must distinguish between directory and non-directory information.
- **HIPAA-FERPA Intersection**: When clinical records are maintained by a school (e.g., school psychologist assessment records), they may be classified as educational records under FERPA rather than healthcare records under HIPAA. The domain model must handle this regulatory overlap correctly.

## Disability Rights: IDEA and ADA

### IDEA (Individuals with Disabilities Education Act)

IDEA creates affirmative obligations for schools to identify, evaluate, and serve students with disabilities, including those with ADHD.

- **Child Find Obligation**: Schools must identify and evaluate students suspected of having disabilities. The domain model must support referral tracking and evaluation timeline compliance (60 calendar days from consent to eligibility determination in most states).
- **Free Appropriate Public Education (FAPE)**: Students eligible under IDEA are entitled to specially designed instruction and related services at no cost. The Accommodation Plan aggregate must enforce FAPE requirements.
- **Procedural Safeguards**: Parents must receive prior written notice of proposed changes to identification, evaluation, placement, or services. The domain model must track notice requirements and parent consent status.
- **Least Restrictive Environment (LRE)**: Students must be educated with non-disabled peers to the maximum extent appropriate. The Accommodation Plan must document LRE considerations.
- **Transition Planning**: For students aged 16 and older, the IEP must include transition goals for post-secondary education, employment, and independent living.

### ADA (Americans with Disabilities Act)

The ADA provides broad disability rights protections that supplement IDEA and Section 504:

- **Reasonable Accommodation**: Employers and post-secondary institutions must provide reasonable accommodations for individuals with ADHD. The domain model's accommodation concepts extend beyond K-12 education to workplace and higher education settings.
- **Undue Hardship Defense**: Accommodation requests can be denied only if they would impose an undue hardship on the institution. The domain model must support documentation of accommodation requests and responses.

## Controlled Substance Regulations

### DEA Scheduling Requirements

Stimulant medications used to treat ADHD are classified as Schedule II controlled substances under the Controlled Substances Act.

- **Prescription Limitations**: Schedule II prescriptions cannot be refilled; a new prescription is required each time. The Medication Regimen aggregate must enforce this constraint.
- **Prescribing Authority**: Only practitioners with valid DEA registration can prescribe Schedule II substances. The Prescriber entity must include DEA registration validation.
- **Quantity Limits**: Many states limit Schedule II prescriptions to a 30-day supply. The domain model must enforce jurisdiction-specific quantity limits.
- **E-Prescribing Requirements**: Federal law requires electronic prescribing of controlled substances (EPCS) in many contexts. The medication management workflow must support EPCS compliance.
- **State-Level Regulations**: Individual states may impose additional requirements such as prescription drug monitoring program (PDMP) checks before prescribing. The domain model must accommodate state-specific regulatory variations.

## Informed Consent

### Assessment Consent

- **Evaluation Consent**: Written parental consent is required before conducting an initial evaluation for IDEA eligibility. The Patient Assessment aggregate must track consent status before assessment can proceed.
- **Re-evaluation Consent**: Consent requirements for re-evaluations vary by jurisdiction and circumstance.

### Treatment Consent

- **Medication Consent**: Informed consent for medication must document the medication name, expected benefits, potential risks and side effects, alternative treatments, and the right to refuse. The Medication Regimen aggregate must track informed consent status.
- **Behavioral Treatment Consent**: Consent for behavioral interventions must describe the treatment approach, expected outcomes, potential risks, and alternatives.

### Minor Consent and Assent

- **Parental Consent**: For patients under 18, parents or legal guardians provide consent for assessment and treatment.
- **Patient Assent**: Age-appropriate assent from the minor patient is a clinical best practice and required in many research contexts.
- **Mature Minor Doctrine**: Some jurisdictions allow minors above a certain age to consent to certain mental health treatments without parental involvement.

## Compliance Enforcement in the Domain Model

Regulatory compliance is enforced through aggregate invariants (e.g., no medication prescription without valid consent), domain events (e.g., ConsentObtained event enabling downstream processes), value objects (e.g., ConsentStatus with legally defined states), and domain services (e.g., ComplianceVerificationService that checks regulatory requirements before operations proceed).

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Health Insurance Portability and Accountability Act of 1996 (HIPAA), 45 CFR Parts 160, 162, 164.
- Family Educational Rights and Privacy Act (FERPA), 20 U.S.C. 1232g; 34 CFR Part 99.
- Individuals with Disabilities Education Act (IDEA), 20 U.S.C. 1400 et seq.
- Americans with Disabilities Act of 1990 (ADA), 42 U.S.C. 12101 et seq.
- Controlled Substances Act, 21 U.S.C. 801 et seq.
