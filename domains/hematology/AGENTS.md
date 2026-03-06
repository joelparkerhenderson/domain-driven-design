# Hematology Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to hematology systems, encompassing blood cell disorders, coagulation management, hematologic malignancies, transfusion medicine, bone marrow transplantation, and laboratory hematology.

## Bounded Contexts

1. Blood Cell Disorders Context - anemia classification, hemoglobinopathy management, white blood cell disorders, platelet disorders, peripheral blood smear interpretation
2. Coagulation Management Context - bleeding disorder diagnosis, thrombophilia evaluation, anticoagulation therapy management, coagulation factor assays, thrombotic event tracking
3. Hematologic Malignancies Context - leukemia classification, lymphoma staging, myeloma management, myeloproliferative neoplasms, chemotherapy protocol administration
4. Transfusion Medicine Context - blood typing and crossmatching, component therapy selection, transfusion reaction monitoring, blood product inventory management, massive transfusion protocols
5. Bone Marrow Transplantation Context - donor matching and selection, conditioning regimen planning, engraftment monitoring, graft-versus-host disease management, post-transplant surveillance
6. Laboratory Hematology Context - complete blood count analysis, flow cytometry, coagulation panel testing, bone marrow biopsy processing, special staining and cytogenetics

## Domain Principles

- Model using shared ubiquitous language between hematologists, hematopathologists, transfusion medicine specialists, oncology nurses, laboratory technologists, and system developers.
- Group the system into bounded contexts reflecting hematology subspecialties and clinical workflows.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow ASH, CAP, and AABB guideline-directed terminology and clinical pathways.
- Maintain HIPAA compliance, blood safety regulations, and patient safety as cross-cutting concerns.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- blood-cell-disorders-context
- coagulation-management-context
- hematologic-malignancies-context
- transfusion-medicine-context
- bone-marrow-transplantation-context
- laboratory-hematology-context
- event-driven-architecture
- integration-patterns
- hematology-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Hoffbrand, A. Victor, and Paul A.H. Moss. Hoffbrand's Essential Haematology. Wiley-Blackwell, 2019.
- Greer, John P., et al. Wintrobe's Clinical Hematology. Wolters Kluwer, 2018.
