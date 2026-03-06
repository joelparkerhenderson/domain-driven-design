# Hematology Domain - Tasks

## Strategic Design

- [ ] Identify core, supporting, and generic subdomains for hematology.
- [ ] Define six bounded contexts aligned with hematology subspecialties and laboratory workflows.
- [ ] Create context map showing relationships between bounded contexts.
- [ ] Establish ubiquitous language with domain experts.
- [ ] Design anti-corruption layers for external system integration (EHR, LIS, blood bank management).

## Tactical Design

- [ ] Model aggregates for each bounded context.
- [ ] Define entities with unique identities within hematology workflows (patients, blood products, treatment protocols, transplant donors).
- [ ] Design value objects for immutable hematology data (CBC values, coagulation results, blood types, WHO disease classifications).
- [ ] Identify domain events that cross bounded context boundaries (TransfusionOrdered, MalignancyDiagnosed, EngraftmentAchieved, BleedingEpisodeRecorded).
- [ ] Define repository abstractions for aggregate persistence.
- [ ] Design domain services for cross-aggregate operations such as antibody identification panels and chemotherapy dose calculations.

## Documentation

- [ ] Document Blood Cell Disorders Context with anemia classification and hemoglobinopathy models.
- [ ] Document Coagulation Management Context with bleeding disorder and anticoagulation therapy models.
- [ ] Document Hematologic Malignancies Context with leukemia/lymphoma staging and chemotherapy protocol models.
- [ ] Document Transfusion Medicine Context with blood typing, crossmatching, and inventory models.
- [ ] Document Bone Marrow Transplantation Context with donor matching and engraftment monitoring models.
