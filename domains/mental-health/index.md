# Mental Health Domain - Domain-Driven Design

## Overview

The Mental Health domain applies Domain-Driven Design principles to model the
complex workflows, clinical reasoning, and regulatory requirements inherent in
mental health care delivery. Mental health systems must coordinate diagnostic
assessment, individualized treatment planning, ongoing therapy management,
crisis response, medication prescribing, and outcomes measurement, each of
which operates under its own rules, vocabularies, and constraints.

DDD provides the strategic and tactical tools to decompose this complexity into
bounded contexts with clear boundaries, shared ubiquitous language, and
well-defined integration patterns. The result is a modular architecture that
reflects how clinicians, patients, and administrators actually think about
mental health care.

## Bounded Contexts

The domain is organized into six bounded contexts:

1. **Clinical Assessment Context** - Handles diagnostic interviews,
   standardized assessments (PHQ-9, GAD-7), screening instruments, and DSM-5
   diagnostic criteria. This context owns the assessment lifecycle from intake
   screening through comprehensive diagnostic evaluation.

2. **Treatment Planning Context** - Manages individualized treatment plans,
   collaborative goal setting, modality selection, care level determination, and
   plan reviews. Treatment plans are living documents that evolve as clinical
   understanding deepens.

3. **Therapy Management Context** - Governs session scheduling, progress note
   documentation, therapeutic modality implementation (CBT, DBT, EMDR), and
   the ongoing therapeutic relationship. This context captures the rhythm of
   clinical encounters.

4. **Crisis Intervention Context** - Addresses acute psychiatric emergencies
   including risk assessment, safety planning, crisis hotline protocols, and
   emergency department coordination. Speed and protocol adherence are
   paramount.

5. **Medication Management Context** - Covers psychotropic prescribing,
   titration schedules, drug interaction checking, adherence monitoring, and
   side effect tracking. This context interfaces heavily with pharmacy systems
   and formularies.

6. **Outcomes Measurement Context** - Tracks symptom changes over time,
   functional assessments, patient-reported outcomes, and treatment
   effectiveness metrics. This context provides the feedback loop that informs
   clinical decision-making.

## Topics

- [Strategic Design](topics/strategic-design/)
- [Bounded Contexts](topics/bounded-contexts/)
- [Context Map](topics/context-map/)
- [Ubiquitous Language](topics/ubiquitous-language/)
- [Subdomains](topics/subdomains/)
- [Anti-Corruption Layer](topics/anti-corruption-layer/)
- [Aggregates](topics/aggregates/)
- [Entities](topics/entities/)
- [Value Objects](topics/value-objects/)
- [Domain Events](topics/domain-events/)
- [Repositories](topics/repositories/)
- [Domain Services](topics/domain-services/)
- [Clinical Assessment Context](topics/clinical-assessment-context/)
- [Treatment Planning Context](topics/treatment-planning-context/)
- [Therapy Management Context](topics/therapy-management-context/)
- [Crisis Intervention Context](topics/crisis-intervention-context/)
- [Medication Management Context](topics/medication-management-context/)
- [Outcomes Measurement Context](topics/outcomes-measurement-context/)
- [Event-Driven Architecture](topics/event-driven-architecture/)
- [Integration Patterns](topics/integration-patterns/)
- [Mental Health Standards](topics/mental-health-standards/)
- [Regulatory Compliance](topics/regulatory-compliance/)

## Key Files

- [CLAUDE.md](CLAUDE.md) - Project instructions
- [plan.md](plan.md) - Strategic plan
- [tasks.md](tasks.md) - Task checklist
- [ubiquitous-language.md](ubiquitous-language.md) - Domain glossary
- [topics/](topics/) - Detailed topic documentation

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Psychiatric Association. Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition (DSM-5). APA Publishing, 2013.
- Kroenke, K., Spitzer, R.L., Williams, J.B. "The PHQ-9: Validity of a Brief Depression Severity Measure." Journal of General Internal Medicine, 2001.
- Spitzer, R.L., Kroenke, K., Williams, J.B., Lowe, B. "A Brief Measure for Assessing Generalized Anxiety Disorder: The GAD-7." Archives of Internal Medicine, 2006.
