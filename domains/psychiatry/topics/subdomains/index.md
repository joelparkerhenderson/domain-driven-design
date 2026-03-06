# Subdomains in the Psychiatry Domain

## Overview

Subdomains classify areas of a domain by their strategic importance to the organization. In Domain-Driven Design, subdomains are categorized as core, supporting, or generic. This classification guides investment decisions: core subdomains receive the deepest modeling effort and most experienced developers, supporting subdomains receive adequate but not maximal investment, and generic subdomains use off-the-shelf or simplified solutions where possible. In the psychiatry domain, this classification reflects the clinical complexity and competitive differentiation of each area.

## Core Subdomains

Core subdomains represent the highest clinical complexity, require the deepest domain expertise, and provide the greatest competitive differentiation for a psychiatry system.

### Diagnostic Assessment

Psychiatric diagnosis is inherently complex. It requires synthesizing information from clinical interviews, patient history, collateral sources, standardized instruments, and clinical judgment. The DSM-5-TR contains hundreds of diagnostic categories with overlapping symptom profiles, making differential diagnosis a sophisticated clinical reasoning process. No off-the-shelf solution can adequately model this complexity. The diagnostic assessment subdomain requires custom modeling with deep collaboration between psychiatrists and developers.

Key complexity drivers:
- Multi-axial diagnostic formulation
- Comorbidity patterns requiring simultaneous consideration
- Cultural and contextual factors influencing diagnostic interpretation
- Longitudinal diagnostic evolution as symptoms change over time

### Medication Management

Psychopharmacology involves complex decision-making about medication selection, dosing, combination therapy, and monitoring. Psychiatrists must consider drug mechanisms of action, receptor profiles, pharmacokinetic interactions, patient genetics, comorbid medical conditions, and regulatory requirements (controlled substance scheduling). This subdomain requires custom modeling because off-the-shelf pharmacy systems do not capture the clinical reasoning specific to psychiatric prescribing.

Key complexity drivers:
- Drug-drug interaction assessment across multiple psychotropic classes
- Pharmacogenomic-guided prescribing personalization
- Polypharmacy risk stratification and simplification
- Controlled substance monitoring and compliance

## Supporting Subdomains

Supporting subdomains enable core clinical activities with specialized workflows that are complex but more standardized than core subdomains.

### Psychotherapy

Psychotherapy delivery follows evidence-based protocols with defined session structures, intervention sequences, and progress criteria. While clinically important, the workflows are more standardized than diagnostic assessment or medication management. Treatment protocols for CBT, DBT, and psychodynamic therapy are well-documented and can be modeled using structured protocol templates. The supporting classification does not diminish clinical importance; it reflects that the modeling patterns are more predictable.

### Crisis Management

Crisis management follows established emergency protocols with defined decision trees for risk assessment, intervention, and disposition. While the clinical stakes are extremely high, the workflows are protocol-driven and more standardized than core subdomains. Risk assessment tools (Columbia Protocol, SAD PERSONS) provide structured frameworks that can be modeled with less custom development.

### Rehabilitation Services

Psychosocial rehabilitation follows established models (Individual Placement and Support for employment, Housing First for residential support) with defined service categories and milestone tracking. The workflows are important but follow patterns common to social service delivery systems, making them less unique to psychiatry specifically.

## Generic Subdomains

Generic subdomains apply general patterns that are not specific to psychiatry and can leverage existing solutions or frameworks.

### Outcomes Measurement

Clinical outcome measurement follows general measurement science patterns: administer instruments, calculate scores, compute change indices, and generate reports. While the specific instruments are psychiatric (PHQ-9, GAD-7, PANSS), the measurement and reporting mechanics are generic across healthcare domains. This subdomain can adapt frameworks from general healthcare quality measurement with psychiatric instrument configurations.

## Investment Implications

| Subdomain | Classification | Modeling Depth | Team Expertise |
|---|---|---|---|
| Diagnostic Assessment | Core | Maximum | Senior domain experts + developers |
| Medication Management | Core | Maximum | Senior domain experts + developers |
| Psychotherapy | Supporting | Moderate | Domain experts + developers |
| Crisis Management | Supporting | Moderate | Domain experts + developers |
| Rehabilitation Services | Supporting | Moderate | Domain experts + developers |
| Outcomes Measurement | Generic | Standard | Developers with domain consultation |

## Subdomain Evolution

Subdomain classifications are not permanent. As psychiatric practice evolves, subdomains may shift classification. For example, if pharmacogenomic-guided prescribing becomes standard practice and commoditized, medication management might shift from core to supporting. Conversely, if measurement-based care becomes a primary competitive differentiator, outcomes measurement might shift from generic to supporting or even core. The classification should be reviewed annually against the organization's strategic priorities.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distillation and core domain.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomains.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1 on core, supporting, and generic subdomains.
