# Subdomains in Cardiology

## Overview

Subdomain classification is a strategic design technique that categorizes areas of the cardiology domain by their strategic importance and complexity. This classification guides investment decisions about modeling depth, team allocation, and build-versus-buy choices. In cardiology, the classification reflects how different clinical functions contribute to the overall quality of cardiac care delivery.

## Core Subdomains

Core subdomains provide the highest strategic value and competitive differentiation. They embody the complex clinical reasoning that defines quality cardiac care and cannot be outsourced or replaced with generic solutions.

### Clinical Assessment and Decision Support

Clinical assessment is a core subdomain because it captures the nuanced reasoning cardiologists apply when evaluating patients. Risk stratification algorithms, clinical decision support logic, and differential diagnosis pathways require deep domain expertise to model correctly. The accuracy of these models directly impacts patient outcomes and differentiates high-quality cardiac care programs.

Key complexity drivers include multi-factorial risk scoring (incorporating demographics, biomarkers, imaging, and comorbidities), guideline-directed diagnostic pathway selection, and the integration of clinical judgment with algorithmic recommendations.

### Heart Failure Management

Heart failure management qualifies as a core subdomain due to the complexity of GDMT optimization, the longitudinal nature of care, and the high-stakes decision-making involved in escalation to advanced therapies. Modeling the interplay between multiple medication classes, their titration protocols, contraindication checking, and side-effect monitoring represents irreducible clinical complexity.

The domain model must capture the four pillars of GDMT (beta-blockers, ACE inhibitors/ARBs/ARNI, mineralocorticoid receptor antagonists, SGLT2 inhibitors), titration schedules, target doses, and the clinical decision logic for when to add, adjust, or discontinue therapies.

## Supporting Subdomains

Supporting subdomains are essential to cardiology operations but follow more standardized workflows. They support the core subdomains by providing critical data and services.

### Diagnostic Imaging

Cardiac imaging follows well-defined acquisition protocols and standardized measurement guidelines (ASE, SCCT, SCMR). While image interpretation requires expertise, the workflow of ordering, acquiring, measuring, and reporting imaging studies is relatively standardized. The imaging subdomain supports clinical decision-making by providing structured data but does not itself embody the core clinical reasoning.

### Interventional Procedures

Interventional cardiology follows protocol-driven workflows for procedural planning, execution, and outcomes tracking. While the procedures themselves require high clinical skill, the domain model for managing procedural information (lesion characteristics, device selection, complications) is more structured and less algorithmically complex than core subdomains.

### Electrophysiology Services

Electrophysiology services involve specialized workflows for arrhythmia diagnosis and device management. While the clinical domain is complex, the information management aspects follow established patterns for study recording, ablation documentation, and device programming protocols.

### Cardiac Rehabilitation

Cardiac rehabilitation follows established Phase I-III protocols with defined exercise prescription parameters, risk factor targets, and outcomes measures. The rehabilitation subdomain supports patient recovery but operates with well-established clinical guidelines that reduce modeling complexity.

## Generic Subdomains

Generic subdomains handle common administrative and operational functions that are not specific to cardiology. These can leverage existing solutions, commercial off-the-shelf products, or shared organizational services.

- **Patient Demographics**: Name, contact information, insurance data.
- **Scheduling**: Appointment booking, resource allocation, calendar management.
- **Billing and Coding**: CPT code submission, insurance claim processing, revenue cycle.
- **Notification Services**: Appointment reminders, result notifications, care team alerts.
- **Document Management**: Report storage, clinical note archival, consent form management.
- **User Authentication and Authorization**: Access control, role management, audit logging.

## Investment Implications

The subdomain classification directly guides development strategy:

- **Core subdomains** receive the highest modeling investment, the most experienced developers, and continuous refinement as clinical guidelines evolve.
- **Supporting subdomains** receive solid modeling with emphasis on data quality and interoperability, but need not capture the full nuance of clinical reasoning.
- **Generic subdomains** use existing solutions wherever possible, minimizing custom development.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distillation.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomain analysis.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1 on analyzing business domains.
- ACC/AHA 2022 Guideline for the Diagnosis and Management of Heart Failure.
