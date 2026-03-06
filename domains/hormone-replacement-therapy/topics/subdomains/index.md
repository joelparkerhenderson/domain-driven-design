# Subdomains in the Hormone Replacement Therapy Domain

Subdomains classify areas of a domain by their strategic importance to the business. Domain-Driven Design identifies three types: core subdomains that provide competitive advantage and require custom solutions, supporting subdomains that are necessary but do not differentiate the system, and generic subdomains that solve common problems addressable by off-the-shelf solutions. In the hormone replacement therapy (HRT) domain, subdomain classification guides investment decisions and modeling depth.

## Purpose

Not all parts of the HRT domain warrant the same level of design effort. Core subdomains demand the deepest modeling because they represent the unique clinical intelligence that differentiates a comprehensive HRT system from basic prescription management. Supporting subdomains require careful modeling but follow more established patterns. Generic subdomains can often leverage existing solutions with configuration rather than custom development.

## Core Subdomains

### Hormone Protocol Management

The Hormone Protocol Context is a core subdomain because it embodies the specialized clinical decision-making that translates patient assessments into individualized treatment plans. The complexity of matching hormone formulations, delivery methods, dosing schedules, and cycling patterns to individual patient profiles represents domain knowledge that is difficult to acquire and provides significant clinical value. The protocol selection algorithms, formulation catalogs, and dosing rules encode years of endocrinological expertise.

### Treatment Optimization

The Treatment Optimization Context is a core subdomain because it performs the analytical reasoning that drives treatment improvement over time. Dose titration algorithms, formulation switching logic, and combination therapy planning require deep understanding of pharmacokinetics, clinical pharmacology, and patient response patterns. This subdomain captures the iterative clinical reasoning that distinguishes expert HRT management from static prescribing.

## Supporting Subdomains

### Clinical Assessment

The Clinical Assessment Context is a supporting subdomain. While essential for the treatment lifecycle, clinical assessment follows relatively standardized patterns: administer validated questionnaires, collect laboratory baselines, screen against contraindication lists, and produce candidacy determinations. The assessment process is important but does not require the same depth of custom modeling as protocol management or treatment optimization. Many of its components, such as symptom scoring instruments and contraindication databases, exist as established clinical tools.

### Side Effect Management

The Side Effect Management Context is a supporting subdomain. Adverse event tracking, contraindication management, and risk stratification are critical for patient safety but follow patterns common to pharmacovigilance across many therapeutic areas. The domain-specific content lies in the HRT-specific contraindication rules and risk factor weightings rather than in novel tracking mechanisms. The underlying tracking, classification, and reporting workflows are well-established in clinical practice.

## Generic Subdomains

### Lab Monitoring

The Lab Monitoring Context is closer to a generic subdomain because laboratory test ordering, result capture, and trend analysis are common patterns in clinical information systems. The HRT-specific content resides in the therapeutic target ranges and monitoring schedules, which are configured into a generic monitoring framework rather than requiring custom modeling. Standard laboratory information system integration patterns apply with minimal HRT-specific adaptation.

### Outcomes Tracking

The Outcomes Tracking Context is also closer to a generic subdomain. Longitudinal outcome measurement, quality of life scoring, and efficacy evaluation are general clinical research patterns applicable across therapeutic domains. The HRT-specific aspects are the particular outcome measures and benchmarks, which configure a general outcomes framework rather than requiring novel modeling approaches.

## Subdomain Interactions

Core subdomains depend on supporting and generic subdomains for input data but retain the most complex domain logic internally. Treatment Optimization (core) consumes data from Lab Monitoring (generic) and Side Effect Management (supporting) but performs its own analytical reasoning. Hormone Protocol (core) consumes data from Clinical Assessment (supporting) but applies its own protocol selection algorithms.

## Investment Implications

Core subdomains receive the highest investment in domain expert collaboration, model refinement, and testing rigor. They justify the use of rich domain models with aggregates, entities, value objects, and domain events. Supporting subdomains may use simpler modeling approaches while maintaining domain accuracy. Generic subdomains may adopt existing solutions or frameworks with configuration, reducing custom development effort.

## Evolution

Subdomain classifications are not permanent. As HRT clinical knowledge advances, a supporting subdomain may become core if new clinical evidence demands more sophisticated modeling. For example, if pharmacogenomic testing becomes central to HRT prescribing, the Clinical Assessment Context might evolve from a supporting to a core subdomain as genetic interpretation logic becomes a primary differentiator.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1 on analyzing business domains.
- Millett, Scott and Nick Tune. Patterns, Principles, and Practices of Domain-Driven Design. Wiley, 2015.
