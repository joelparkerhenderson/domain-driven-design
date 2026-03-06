# Subdomains in the Rheumatology Domain

## Overview

Subdomains classify areas of the domain by their strategic importance to the organization. In Domain-Driven Design, subdomains are categorized as core, supporting, or generic. This classification guides where to invest the most design effort and expertise. In the rheumatology domain, the classification reflects which clinical capabilities provide differentiation, which support clinical operations, and which can leverage standard solutions.

## Core Subdomains

Core subdomains represent the areas where the rheumatology system provides its greatest value and competitive differentiation. These require the deepest domain expertise and the most sophisticated modeling.

### Clinical Assessment and Disease Activity Scoring

The ability to accurately capture and calculate disease activity scores is fundamental to rheumatology practice. DAS28, CDAI, SDAI, and RAPID3 calculations embed complex clinical logic including component weighting, mathematical transformations (DAS28 uses square roots and natural logarithms), and threshold-based interpretation. This scoring logic is unique to rheumatology and represents core clinical knowledge.

### Autoimmune Disease Classification and Management

Applying ACR/EULAR classification criteria, managing treat-to-target strategies, detecting disease flares, and guiding treatment escalation represent the core intellectual property of rheumatology practice. The rules for classifying rheumatoid arthritis (2010 ACR/EULAR criteria with joint involvement, serology, acute phase reactants, and duration) are complex and clinically critical.

### Biologic Therapy Decision Support

Selecting appropriate biologic or targeted synthetic DMARDs based on disease characteristics, prior treatment history, comorbidities, and safety profiles requires sophisticated domain logic. Biosimilar switching protocols, pre-treatment screening requirements, and drug-specific monitoring schedules are complex, high-stakes decisions that differentiate expert rheumatology care.

## Supporting Subdomains

Supporting subdomains enable core clinical functions but do not themselves represent primary clinical differentiation. They require custom development but with less domain complexity than core subdomains.

### Joint Disease Management

While gout management and joint injection therapy require clinical expertise, these workflows are more standardized and less complex than autoimmune disease management. Crystal arthropathy diagnosis follows well-established criteria, and urate-lowering therapy targets are clearly defined (serum urate below 6 mg/dL). This subdomain supports the clinical mission without driving differentiation.

### Pain Management Coordination

Pain management in rheumatology involves referral coordination, tracking therapy progress, and documenting multimodal interventions. While important for patient care, the pain management workflows are not unique to rheumatology. Physical therapy referral and progress tracking follow patterns common across musculoskeletal medicine.

### Outcomes Tracking and Reporting

Longitudinal outcome tracking using standardized instruments (HAQ-DI, radiographic scoring, PROMs) supports clinical decision-making and quality reporting. The tracking logic is important but follows well-defined measurement protocols established by OMERACT and regulatory bodies. The value lies in aggregation and longitudinal analysis rather than novel domain logic.

## Generic Subdomains

Generic subdomains provide necessary functionality that is not specific to rheumatology and can be addressed with off-the-shelf or standard solutions.

### Patient Demographics and Identity

Managing patient identity, demographics, contact information, and insurance details is essential but not specific to rheumatology. Standard patient management systems handle these concerns.

### Scheduling and Appointments

Appointment scheduling for clinic visits, infusion sessions, and therapy appointments follows general healthcare scheduling patterns. While infusion scheduling has some specific requirements (chair time, pre-medication timing), these are configuration concerns rather than deep domain logic.

### Document Management

Storing and retrieving clinical documents, lab reports, and imaging studies is a generic capability. Standard document management and clinical data repository solutions apply.

### User Authentication and Authorization

Access control, role-based permissions, and audit logging follow standard healthcare information security patterns and can leverage generic identity management solutions.

## Strategic Investment Implications

Core subdomains warrant the strongest development teams, the most thorough domain expert engagement, and the most sophisticated modeling techniques. These are where EventStorming sessions, detailed aggregate design, and continuous refactoring provide the most value.

Supporting subdomains can use simpler modeling approaches. Active Record or Transaction Script patterns may suffice where the domain logic is straightforward. Development teams should be competent but need not be domain experts.

Generic subdomains should be addressed with purchased or open-source solutions wherever possible. Custom development in generic subdomains is a misallocation of strategic resources.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1.
- Tune, Nick. "Core Domain Patterns." Architecture Modernization, Manning, 2024.
