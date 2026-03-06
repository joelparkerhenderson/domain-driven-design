# Hormone Replacement Therapy Domain - CLAUDE.md

This domain applies Domain-Driven Design (DDD) to hormone replacement therapy (HRT) systems.

## Scope

Clinical assessment, hormone protocol management, lab monitoring, side effect management, treatment optimization, and outcomes tracking.

## Bounded Contexts

1. Clinical Assessment Context - symptom evaluation, hormone panel interpretation, candidacy screening
2. Hormone Protocol Context - HRT regimens (estrogen, progesterone, testosterone), delivery methods, dosing
3. Lab Monitoring Context - hormone levels, metabolic panels, bone density, cardiovascular markers
4. Side Effect Management Context - adverse event tracking, risk mitigation, contraindication management
5. Treatment Optimization Context - dose titration, formulation adjustment, combination therapy
6. Outcomes Tracking Context - symptom resolution, quality of life, long-term health outcomes

## Conventions

- Use ubiquitous language consistently across all documentation and models.
- Each bounded context maintains its own domain model and terminology.
- Business logic remains pure and independent of infrastructure concerns.
- All topic documentation resides in the topics/ directory with individual index.md files.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
