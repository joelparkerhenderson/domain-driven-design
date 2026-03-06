# Mental Health Domain - DDD Project Instructions

This domain applies Domain-Driven Design (DDD) to mental health systems,
covering clinical assessment, treatment planning, therapy management, crisis
intervention, medication management, and outcomes measurement.

## Bounded Contexts

1. Clinical Assessment Context - diagnostic interviews, standardized assessments (PHQ-9, GAD-7), DSM-5 criteria
2. Treatment Planning Context - individualized treatment plans, goal setting, modality selection, care levels
3. Therapy Management Context - session scheduling, progress notes, therapeutic modalities (CBT, DBT, EMDR)
4. Crisis Intervention Context - risk assessment, safety planning, crisis hotline, emergency protocols
5. Medication Management Context - psychotropic prescribing, titration, drug interactions, adherence
6. Outcomes Measurement Context - symptom tracking, functional assessment, patient-reported outcomes

## Domain Principles

- Use ubiquitous language shared by clinicians, therapists, psychiatrists, and software developers.
- Model business logic purely; do not embed infrastructure concerns such as databases or UI.
- Maintain strict bounded context boundaries to prevent concept leakage between contexts.
- Follow HIPAA and 42 CFR Part 2 regulations for behavioral health information.
- Apply CQRS where read and write concerns diverge, such as clinical dashboards versus intake workflows.

## Project Structure

- plan.md - strategic plan for the domain
- tasks.md - task checklist
- index.md - domain overview and documentation hub
- ubiquitous-language.md - canonical glossary of domain terms
- topics/ - detailed documentation per topic

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
