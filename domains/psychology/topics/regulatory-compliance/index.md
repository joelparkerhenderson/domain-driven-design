# Regulatory Compliance in the Psychology Domain

## Overview

Regulatory compliance encompasses the legal, ethical, and governance requirements that shape the design of the psychology domain model. Psychology practice operates under a layered regulatory framework that includes federal health privacy laws, state licensure requirements, professional ethical codes, institutional policies, and payer-specific rules. These requirements are not external add-ons to the domain model -- they fundamentally shape how clinical data is structured, accessed, stored, and communicated across bounded contexts.

In Domain-Driven Design, regulatory requirements inform aggregate boundaries, entity lifecycle rules, value object validation, event content, and anti-corruption layer design. A domain model that ignores regulatory constraints will fail to serve its clinical users, who are personally liable for regulatory violations.

## HIPAA (Health Insurance Portability and Accountability Act)

HIPAA establishes the federal framework for protecting health information in the United States. The Privacy Rule governs who can access protected health information (PHI) and under what circumstances. The Security Rule specifies administrative, physical, and technical safeguards for electronic PHI. The Breach Notification Rule requires notification when PHI is compromised.

HIPAA shapes the psychology domain model in several ways. Access to clinical data within each bounded context must be restricted to authorized personnel with a legitimate clinical or operational need. The Client entity carries PHI that must be protected throughout its lifecycle. Domain events that cross context boundaries must not carry more PHI than the receiving context needs. The anti-corruption layer between clinical contexts and the Research Methods Context must de-identify data to comply with HIPAA's research provisions.

The minimum necessary standard requires that only the minimum amount of PHI needed for a specific purpose is disclosed. This principle directly influences event design. An AssessmentCompleted event should carry a clinical summary, not the full assessment record, unless the full record is necessary for the consuming context's function.

## APA Ethical Principles

The American Psychological Association's Ethical Principles of Psychologists and Code of Conduct establishes professional ethical standards that carry legal weight in many jurisdictions. Key principles that shape the domain model include:

Informed Consent (Standard 3.10): Clients must be informed about the nature and purpose of assessment and treatment. The domain model captures informed consent through the Consent Record value object, which documents what information was provided, the client's understanding, and the voluntary nature of the consent. The Assessment aggregate and Treatment Plan aggregate should not proceed beyond initial stages without a valid Consent Record.

Confidentiality (Standard 4): Psychologists have a duty to protect confidential information. The domain model must enforce access controls that limit who can view clinical data. Cross-context data sharing must follow the minimum necessary principle. Exceptions to confidentiality, such as mandated reporting of child abuse or imminent danger, must be explicitly modeled.

Assessment Standards (Standard 9): Psychologists must use assessment instruments that are appropriate for the client's characteristics. The Test Selection Service should validate that chosen instruments have adequate psychometric properties for the client's demographic group. Assessment interpretation must consider factors that could affect test validity.

Record Keeping (Standard 6): Psychologists must maintain appropriate records. The domain model must support record retention policies that comply with state law and APA guidelines. Records must be maintained for the required retention period and must be accessible for authorized requests.

## State Licensure Laws

Psychology practice is regulated at the state level through licensure laws that vary by jurisdiction. These laws define who may practice psychology, the scope of practice, supervision requirements, continuing education requirements, and record-keeping obligations.

The domain model must accommodate jurisdictional variation without hard-coding specific state requirements. This is achieved through configurable validation rules and policy objects that can be adjusted for different jurisdictions. A Licensure value object associated with the Therapist entity can capture jurisdiction-specific credentials and scope of practice.

## IRB Regulations (45 CFR 46)

The federal Common Rule (45 CFR Part 46) governs the protection of human subjects in research. It requires Institutional Review Board approval for research involving human participants, informed consent from participants, ongoing monitoring of research risks, and special protections for vulnerable populations.

The Research Study aggregate enforces these requirements as invariants. Data collection cannot begin without IRB approval. Participant enrollment requires documented informed consent. Adverse events must be reported. Protocol modifications must be reviewed before implementation.

## Mandated Reporting

Psychologists in all U.S. states are mandated reporters of child abuse and neglect. Many states extend mandated reporting to elder abuse, dependent adult abuse, and threats of imminent harm. The domain model must support the documentation and reporting of mandated report events.

The CrisisEventRecorded domain event in the Therapeutic Intervention Context can trigger mandated reporting workflows. The domain model captures the clinical observations that give rise to a duty to report, the report itself, and the outcome of the report. This documentation protects both the client and the clinician.

## Payer and Accreditation Requirements

Insurance payers impose requirements on treatment authorization, session limits, documentation standards, and outcome reporting. Accreditation bodies like the Joint Commission and CARF establish quality standards for behavioral health organizations.

These requirements influence the domain model through Treatment Plan aggregate rules about authorization and review frequency, Session entity documentation standards, and Outcomes Measurement Context reporting formats. The anti-corruption layer between the psychology domain and payer systems translates clinical concepts into authorization and claims language.

## Data Retention and Disposal

Regulatory requirements specify how long clinical records must be retained and how they must be disposed of when retention periods expire. Adult records are typically retained for 7 years after the last date of service. Records for minors may need to be retained until the client reaches the age of majority plus an additional retention period.

The domain model must support record lifecycle management, including retention tracking, archival, and secure disposal. This applies across all bounded contexts that maintain clinical or research records.

## Compliance as Domain Logic

Regulatory compliance is not an infrastructure concern -- it is domain logic. The rules governing who can access what data, when consent is required, how records are retained, and what must be reported are business rules that belong in the domain model. They should be expressed in the ubiquitous language and enforced by aggregate invariants, not relegated to application or infrastructure layers.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- U.S. Department of Health and Human Services. (2013). HIPAA Administrative Simplification Regulation Text.
- American Psychological Association. (2017). Ethical Principles of Psychologists and Code of Conduct.
- Office for Human Research Protections. (2018). Federal Policy for the Protection of Human Subjects (Common Rule).
