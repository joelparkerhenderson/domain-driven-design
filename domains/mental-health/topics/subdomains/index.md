# Subdomains in the Mental Health Domain

## Overview

In Domain-Driven Design, subdomains classify areas of a domain by their
strategic importance to the organization. Eric Evans identifies three types:
core subdomains (the competitive differentiator), supporting subdomains
(necessary but not differentiating), and generic subdomains (common
capabilities available off the shelf). Correct classification drives
investment decisions: core subdomains warrant the most talented developers
and the most sophisticated modeling, while generic subdomains should be
purchased or reused rather than custom-built.

In the mental health domain, subdomain classification reflects which aspects
of mental health care delivery create the most value and which follow
standardized patterns that are common across healthcare systems.

## Core Subdomains

### Clinical Assessment

Clinical assessment is a core subdomain because the quality and accuracy of
diagnostic evaluation directly determines treatment outcomes. Mental health
assessment is inherently complex: it requires integrating patient-reported
symptoms, clinician observations, standardized instrument scores, and
diagnostic criteria into a coherent clinical formulation. The way a system
supports this integration is a genuine differentiator. Organizations that
model assessment workflows with precision can reduce diagnostic errors,
improve inter-rater reliability, and accelerate time to appropriate treatment.

### Treatment Planning

Treatment planning is a core subdomain because individualized, evidence-based
treatment plans are the foundation of effective mental health care. The
ability to match patients to the right combination of therapeutic modalities,
care levels, and goals based on their specific diagnoses, preferences, and
circumstances is what separates excellent care from formulaic approaches.
Modeling the treatment plan as a living, evolving document that responds to
clinical feedback requires deep domain expertise.

## Supporting Subdomains

### Therapy Management

Therapy management is a supporting subdomain. While essential to care
delivery, the workflows of scheduling sessions, documenting progress notes,
and implementing therapeutic modalities follow well-established clinical
patterns. The value lies in reliable execution rather than innovative
modeling. Therapy management supports the core subdomains by providing
the infrastructure through which treatment plans are carried out.

### Crisis Intervention

Crisis intervention is a supporting subdomain. Crisis protocols are
standardized by organizations such as the Suicide Prevention Resource Center
and the Joint Commission. The primary modeling challenge is ensuring that
protocols are followed correctly and rapidly rather than inventing new
approaches. Crisis intervention supports the entire system by providing
a safety net that protects patients during acute episodes.

## Generic Subdomains

### Medication Management

Medication management is a generic subdomain. Psychotropic prescribing
follows standardized protocols, drug interaction databases are commercially
available, and pharmacy integration patterns are well-established through
standards like NCPDP SCRIPT and HL7 FHIR MedicationRequest. While medication
management must be accurate and safe, the modeling challenges are largely
solved problems that can leverage existing pharmaceutical knowledge bases
and formulary systems.

### Outcomes Measurement

Outcomes measurement is a generic subdomain. The science of measuring
treatment outcomes uses validated instruments (PHQ-9, GAD-7, WHODAS 2.0),
established psychometric methods, and standardized reporting frameworks
(HEDIS, NQF measures). The measurement methodology is domain-agnostic;
what changes is the specific instruments and metrics, not the underlying
approach to longitudinal data collection and analysis.

## Investment Implications

The subdomain classification drives several architectural decisions:

- Core subdomains (Clinical Assessment, Treatment Planning) should receive
  custom, in-house development with deep collaboration between clinicians
  and developers. These models should use rich, expressive domain patterns.
- Supporting subdomains (Therapy Management, Crisis Intervention) can use
  simpler patterns and may benefit from established frameworks, but still
  require domain-specific customization.
- Generic subdomains (Medication Management, Outcomes Measurement) should
  leverage existing solutions, commercial products, or open standards
  wherever possible, with anti-corruption layers to integrate them.

## Subdomain Boundaries and Bounded Contexts

In this domain, each subdomain maps one-to-one with a bounded context. This
is a clean alignment, but it is not always the case in DDD. Sometimes a
single subdomain spans multiple bounded contexts, or multiple subdomains
coexist within one bounded context. The one-to-one mapping here reflects
the natural clinical workflow boundaries in mental health care.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 3 on subdomain classification.
