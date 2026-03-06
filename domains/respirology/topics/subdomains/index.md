# Subdomains in the Respirology Domain

## Overview

Subdomain classification is a strategic design activity that categorizes areas of the domain by their business importance and complexity. In Domain-Driven Design, subdomains are classified as core, supporting, or generic. This classification guides investment decisions: core subdomains receive the most development attention and custom modeling, supporting subdomains require solid implementation but less innovation, and generic subdomains are best handled by off-the-shelf solutions or external services.

In the respirology domain, subdomain classification reflects which areas of respiratory medicine contain the most specialized clinical logic and which areas follow standardized protocols common across healthcare.

## Core Subdomains

Core subdomains contain the business logic that provides the greatest clinical and competitive value. They require deep domain expertise and custom development. In the respirology domain, three areas qualify as core subdomains.

### Clinical Assessment

Clinical assessment is a core subdomain because it encodes the specialized reasoning that pulmonologists use to evaluate respiratory status. Risk stratification algorithms, symptom scoring interpretation, and the integration of examination findings into a clinical picture represent irreplaceable domain expertise. The accuracy of downstream decisions depends on the quality of the assessment model.

### Respiratory Diagnostics

Respiratory diagnostics is a core subdomain because the interpretation of pulmonary function tests, imaging findings, and bronchoscopy results requires specialized clinical logic. Determining whether spirometry results indicate obstruction, restriction, or a mixed pattern involves rules that encode decades of pulmonary medicine knowledge. Automated interpretation support and quality assurance for test results are high-value capabilities.

### Chronic Disease Management

Chronic disease management is a core subdomain because it captures the longitudinal clinical reasoning required to manage patients with COPD, ILD, bronchiectasis, and sarcoidosis. Action plan logic, exacerbation prediction, treatment escalation rules, and GOLD classification algorithms represent complex domain logic that differentiates a respirology system from a generic chronic care platform.

## Supporting Subdomains

Supporting subdomains are necessary for the system to function but do not represent the primary source of clinical value. They follow well-established protocols and benefit from reliable, straightforward implementation.

### Airway Management

Airway management is a supporting subdomain because the procedures involved (intubation, tracheostomy, airway clearance) follow standardized protocols established by professional societies. While proper documentation and tracking are essential for patient safety, the domain logic is more procedural than algorithmic. The primary requirement is accurate recording and protocol adherence rather than complex decision support.

### Ventilatory Support

Ventilatory support is a supporting subdomain because ventilator management follows established clinical protocols for mode selection, parameter adjustment, and weaning. The logic for alarm handling, parameter validation, and weaning readiness assessment is important but follows published guidelines rather than requiring novel clinical reasoning. Device integration adds technical complexity but not domain complexity.

### Pulmonary Rehabilitation

Pulmonary rehabilitation is a supporting subdomain because rehabilitation programs follow evidence-based structures defined by ATS/ERS guidelines. Exercise prescription, breathing retraining techniques, and outcome measurement use standardized approaches. The value lies in reliable program delivery and outcome tracking rather than novel algorithmic logic.

## Generic Subdomains

Generic subdomains handle concerns that are common across healthcare systems and are not specific to respirology. These should be implemented using existing solutions or external services.

### Patient Identity Management

Managing patient demographics, identifiers, and contact information is a generic concern shared across all clinical domains. Hospital master patient index (MPI) systems or identity services handle this subdomain.

### Scheduling and Appointments

Appointment scheduling for clinic visits, diagnostic tests, and rehabilitation sessions is a generic capability provided by hospital scheduling systems or standard scheduling platforms.

### Billing and Coding

Medical billing, procedure coding (ICD-10, CPT), and insurance claim processing are generic concerns handled by revenue cycle management systems.

### User Authentication and Authorization

Clinician identity verification, role-based access control, and audit logging are generic security concerns handled by identity management platforms.

## Classification Rationale

The classification of subdomains is not fixed. As clinical practice evolves, a supporting subdomain may become core if novel algorithms or decision support capabilities are developed within it. For example, if artificial intelligence-driven ventilator management becomes a key differentiator, the Ventilatory Support subdomain might be reclassified as core.

Conversely, if standardized decision support for COPD management becomes widely available through open-source clinical decision support systems, the Chronic Disease Management subdomain's classification might shift from core toward supporting.

## Investment Implications

- **Core subdomains** should receive the best developers, the most thorough domain modeling, and the most rigorous testing. Custom development is justified.
- **Supporting subdomains** should receive competent implementation with adequate testing. Consider frameworks and patterns that accelerate development without sacrificing quality.
- **Generic subdomains** should be handled by purchasing or integrating existing solutions. Custom development is not justified.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 15 on distillation and core domain.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2 on subdomains.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 3 on subdomain classification and strategic analysis.
