# Regulatory Compliance in the Mental Health Domain

## Overview

Regulatory compliance shapes every aspect of the mental health domain model.
Mental health care operates under multiple overlapping regulatory frameworks
that govern how patient information is stored, shared, and protected; how
treatment is authorized and documented; how medications are prescribed; and
how crises are managed. In Domain-Driven Design, regulatory requirements are
not afterthoughts bolted onto the model; they are domain invariants that
the model must enforce by design.

Failing to model regulatory requirements correctly has consequences beyond
technical debt: it can result in HIPAA violations, patient safety incidents,
loss of accreditation, and legal liability. The domain model must make
compliance the natural outcome of correct system behavior.

## HIPAA (Health Insurance Portability and Accountability Act)

HIPAA establishes national standards for the protection of individually
identifiable health information. All six bounded contexts in the mental
health domain handle protected health information (PHI) and must comply
with HIPAA Privacy, Security, and Breach Notification rules.

Key modeling implications:

- The Privacy Rule restricts how PHI can be used and disclosed. The domain
  model must enforce minimum necessary access: each bounded context should
  only access the PHI it needs for its specific function.
- Psychotherapy notes (as defined in 45 CFR 164.501) receive heightened
  protection. The Therapy Management Context must segregate psychotherapy
  notes from other clinical documentation because they generally cannot be
  disclosed without specific patient authorization, even for treatment,
  payment, or healthcare operations.
- Patient authorization must be modeled as a first-class domain concept.
  The shared kernel for patient identity should include consent and
  authorization status.
- The Security Rule requires administrative, physical, and technical
  safeguards. Event streams, repositories, and anti-corruption layers must
  implement encryption, access controls, and audit logging.

## 42 CFR Part 2 (Confidentiality of Substance Use Disorder Records)

42 CFR Part 2 provides additional protections for substance use disorder
(SUD) treatment records beyond HIPAA. These protections apply whenever a
patient receives SUD treatment from a federally assisted program, which
includes most mental health treatment settings.

Key modeling implications:

- SUD-related information requires explicit patient consent for any
  disclosure, including for treatment purposes. The domain model must track
  SUD-specific consent separately from general HIPAA consent.
- Re-disclosure prohibitions mean that SUD information shared with another
  context or external system must carry a notice prohibiting further
  disclosure. Domain events carrying SUD-related data must include
  re-disclosure restriction metadata.
- The Clinical Assessment Context must segregate SUD screening results
  from general mental health assessment results when necessary to comply
  with Part 2 restrictions.
- Recent regulatory amendments (2024) have aligned some Part 2 provisions
  with HIPAA, but the domain model should still maintain SUD-specific
  consent tracking until the regulatory landscape fully stabilizes.

## Mental Health Parity Laws

The Mental Health Parity and Addiction Equity Act (MHPAEA) requires that
insurance coverage for mental health and substance use conditions be no
more restrictive than coverage for medical and surgical conditions.

Key modeling implications:

- The Treatment Planning Context must be able to document and compare
  treatment limitations (visit limits, prior authorization requirements,
  cost sharing) between mental health and medical coverage.
- The Outcomes Measurement Context may need to produce parity compliance
  data demonstrating that mental health treatment authorization criteria
  are comparable to medical criteria.

## State Mental Health Statutes

State laws govern involuntary commitment, duty to warn, mandatory reporting,
and scope of practice for mental health professionals. These laws vary
significantly by jurisdiction.

Key modeling implications:

- The Crisis Intervention Context must model jurisdiction-specific
  involuntary commitment criteria, hold durations, and judicial review
  requirements. These rules should be configurable rather than hardcoded.
- Duty to warn (Tarasoff-type obligations) varies by state. The domain
  model must support jurisdiction-specific rules for when clinicians are
  required or permitted to breach confidentiality to warn potential victims.
- Mandatory reporting obligations (child abuse, elder abuse, certain
  communicable diseases) must be modeled in the relevant contexts.

## Controlled Substance Regulations

The Medication Management Context must comply with DEA regulations and state
Prescription Drug Monitoring Program (PDMP) requirements for controlled
substances.

Key modeling implications:

- Prescriptions for Schedule II-V medications must include DEA number
  verification.
- Electronic prescribing of controlled substances (EPCS) must meet DEA
  identity proofing and two-factor authentication requirements.
- The domain model must support PDMP queries before controlled substance
  prescribing to check for potential misuse or diversion.
- Prescription quantities and refill rules vary by schedule and state.

## Accreditation Standards

Mental health organizations may be accredited by the Joint Commission, CARF
(Commission on Accreditation of Rehabilitation Facilities), or state
agencies. Accreditation standards affect documentation requirements,
treatment planning timelines, and quality reporting.

Key modeling implications:

- Treatment plan review intervals must comply with accreditation standards.
- Progress note documentation timelines must be configurable to match
  accreditation requirements.
- Crisis intervention protocols must align with accreditation body safety
  standards.

## Compliance as Domain Invariants

The domain model enforces compliance through aggregate invariants:

- The TreatmentPlan aggregate enforces review date requirements.
- The Prescription aggregate enforces controlled substance prescribing rules.
- The CrisisEpisode aggregate enforces documentation requirements for
  involuntary holds.
- The Session aggregate enforces progress note completion timelines.
- The Assessment aggregate enforces finalization requirements before results
  can be shared.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. HIPAA Privacy Rule. 45 CFR Parts 160, 164.
- Substance Abuse and Mental Health Services Administration. 42 CFR Part 2. SAMHSA, 2020.
- U.S. Congress. Mental Health Parity and Addiction Equity Act (MHPAEA). Public Law 110-343, 2008.
- Joint Commission. Behavioral Health Care Accreditation Standards. Joint Commission, 2023.
