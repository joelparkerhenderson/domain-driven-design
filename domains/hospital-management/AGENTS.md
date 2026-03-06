# Hospital Management - Domain-Driven Design

This domain applies Domain-Driven Design (DDD) to hospital management, creating a modular and scalable system by modeling software directly after complex healthcare workflows.

## Bounded Contexts

- **Patient Context** - Registration, demographics, medical history
- **Appointment/Scheduling Context** - Doctor schedules, appointments, resource allocation
- **Clinical/EMR Context** - Diagnostics, lab tests, imaging, treatment plans
- **Billing/Administrative Context** - Invoices, insurance claims, payments
- **Emergency Services Context** - Triage, urgent care, trauma protocols

## Ubiquitous Language

The shared vocabulary between developers and clinicians is defined in `language.md`.

## Directory Structure

- `index.md` - Main documentation entry point
- `plan.md` - Project plan
- `tasks.md` - Task tracking
- `language.md` - Ubiquitous language glossary
- `topics/` - Detailed topic documentation

## Conventions

- Every directory has `index.md` with a symlink `README.md` -> `index.md`
- No images, diagrams, web apps, front-end, or back-end code
- Comprehensive documentation with references and citations
- Ubiquitous language terms: one per line, one-sentence explanation

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
