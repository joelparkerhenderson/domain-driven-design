# Psychiatry Domain - Domain-Driven Design

## Overview

This domain applies Domain-Driven Design (DDD) to psychiatry systems. Psychiatry encompasses the diagnosis, treatment, and prevention of mental disorders, requiring complex clinical workflows that span diagnostic assessment, medication management, psychotherapy, crisis intervention, rehabilitation, and outcomes measurement. DDD provides a structured approach to modeling these workflows by establishing a shared ubiquitous language between psychiatrists, psychologists, nurses, social workers, and software developers, and by decomposing the system into well-defined bounded contexts.

## Bounded Contexts

The psychiatry domain is organized into six bounded contexts, each representing a distinct area of clinical responsibility:

1. **Diagnostic Assessment Context** - Manages psychiatric evaluation workflows including structured clinical interviews, DSM-5-TR diagnostic classification, standardized rating scales (PHQ-9, GAD-7, PANSS), and differential diagnosis processes. This context owns the canonical representation of a patient's diagnostic profile.

2. **Medication Management Context** - Manages psychopharmacological treatment including prescription lifecycle, therapeutic drug monitoring, drug interaction checking, pharmacogenomic-guided prescribing, and polypharmacy risk assessment. This context owns the medication record and prescribing authority workflows.

3. **Psychotherapy Context** - Manages therapeutic interventions including modality selection (CBT, DBT, psychodynamic), session scheduling and documentation, treatment protocol adherence, homework assignments, and therapeutic alliance tracking. This context owns the therapy record and session history.

4. **Crisis Management Context** - Manages psychiatric emergencies including suicide risk assessment (Columbia Protocol), involuntary hold procedures, safety planning, de-escalation protocols, and emergency contact coordination. This context owns crisis event records and safety plans.

5. **Rehabilitation Services Context** - Manages psychosocial rehabilitation including supported employment programs, housing assistance, social skills training, cognitive remediation, and community reintegration planning. This context owns rehabilitation plans and functional milestones.

6. **Outcomes Measurement Context** - Manages clinical outcome tracking including standardized outcome measures, functional assessment scales, patient-reported outcomes, quality metrics, and treatment effectiveness analysis. This context owns outcome data and reporting.

## Subdomains

- **Core Subdomain**: Diagnostic Assessment and Medication Management represent the highest clinical complexity and domain-specific knowledge.
- **Supporting Subdomain**: Psychotherapy, Crisis Management, and Rehabilitation Services support core clinical activities with specialized workflows.
- **Generic Subdomain**: Outcomes Measurement applies general measurement and reporting patterns that can be adapted from other healthcare domains.

## Documentation

- [CLAUDE.md](CLAUDE.md) - Domain configuration and guidelines
- [plan.md](plan.md) - Vision, scope, and milestones
- [tasks.md](tasks.md) - Task tracking
- [ubiquitous-language.md](ubiquitous-language.md) - Domain terminology
- [topics/](topics/index.md) - Detailed topic documentation

## Topics

### Strategic Design
- [Strategic Design](topics/strategic-design/index.md)
- [Bounded Contexts](topics/bounded-contexts/index.md)
- [Context Map](topics/context-map/index.md)
- [Ubiquitous Language](topics/ubiquitous-language/index.md)
- [Subdomains](topics/subdomains/index.md)
- [Anti-Corruption Layer](topics/anti-corruption-layer/index.md)

### Tactical Design
- [Aggregates](topics/aggregates/index.md)
- [Entities](topics/entities/index.md)
- [Value Objects](topics/value-objects/index.md)
- [Domain Events](topics/domain-events/index.md)
- [Repositories](topics/repositories/index.md)
- [Domain Services](topics/domain-services/index.md)

### Bounded Context Details
- [Diagnostic Assessment Context](topics/diagnostic-assessment-context/index.md)
- [Medication Management Context](topics/medication-management-context/index.md)
- [Psychotherapy Context](topics/psychotherapy-context/index.md)
- [Crisis Management Context](topics/crisis-management-context/index.md)
- [Rehabilitation Services Context](topics/rehabilitation-services-context/index.md)
- [Outcomes Measurement Context](topics/outcomes-measurement-context/index.md)

### Cross-Cutting Concerns
- [Event-Driven Architecture](topics/event-driven-architecture/index.md)
- [Integration Patterns](topics/integration-patterns/index.md)
- [Psychiatry Standards](topics/psychiatry-standards/index.md)
- [Regulatory Compliance](topics/regulatory-compliance/index.md)

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- American Psychiatric Association. *Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition, Text Revision (DSM-5-TR)*. APA Publishing, 2022.
- Stahl, Stephen M. *Stahl's Essential Psychopharmacology*. Cambridge University Press, 2021.
- Linehan, Marsha M. *DBT Skills Training Manual*. Guilford Press, 2015.
