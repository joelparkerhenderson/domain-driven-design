# Repositories in the Orthopaedic Domain

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection
of aggregate roots. It encapsulates the logic for storing, retrieving, and searching
aggregates while keeping the domain model free from persistence concerns. In the
orthopaedic domain, repositories provide clean interfaces for accessing clinical,
surgical, and rehabilitation aggregates.

## Purpose

Orthopaedic domain logic must remain independent of how data is stored. Whether
fracture cases are persisted in a relational database, a document store, or an event
store, the domain model interacts with a repository interface that speaks the
ubiquitous language. This separation ensures that the clinical business rules are
not contaminated by SQL queries, ORM configurations, or storage-specific data structures.

## Key Repositories

### Clinical Assessment Repositories

**ClinicalEncounterRepository**: Provides access to Clinical Encounter aggregates.
Operations include retrieving encounters by patient identifier, by date range, by
clinician, or by provisional diagnosis. Supports adding new encounters and updating
existing ones as examination findings are recorded.

**ImagingStudyRepository**: Provides access to imaging study references. Supports
retrieval by accession number, by patient, by anatomical region, and by date.

### Surgical Planning Repositories

**SurgicalPlanRepository**: Provides access to Surgical Plan aggregates. Supports
retrieval by plan identifier, by patient, by surgeon, by scheduled date, and by
approval status. Enables querying for plans in draft state that require review.

**ImplantCatalogRepository**: Provides read-only access to the implant catalog,
supporting queries by manufacturer, implant type, size range, and material. This
repository serves as a reference for implant selection during planning.

### Joint Replacement Repositories

**ArthroplastyCaseRepository**: Provides access to Arthroplasty Case aggregates.
Supports retrieval by case identifier, by patient, by implant serial number, by
date range, and by joint type. Enables longitudinal queries for outcome tracking
across years of follow-up.

**ImplantComponentRepository**: Provides access to individual implant component
records. Supports retrieval by serial number, by catalog number, and by patient.
Critical for implant recall and traceability queries.

### Sports Medicine Repositories

**SportsInjuryCaseRepository**: Provides access to Sports Injury Case aggregates.
Supports retrieval by case identifier, by athlete, by injury type, by sport, and
by return-to-play status. Enables population-level queries for injury pattern analysis.

### Fracture Management Repositories

**FractureCaseRepository**: Provides access to Fracture Case aggregates. Supports
retrieval by case identifier, by patient, by AO/OTA classification, by fixation
method, and by healing status. Enables epidemiological queries on fracture patterns.

### Rehabilitation Repositories

**RehabilitationEpisodeRepository**: Provides access to Rehabilitation Episode
aggregates. Supports retrieval by episode identifier, by patient, by protocol type,
by current phase, and by treating therapist. Enables queries for episodes approaching
milestone deadlines.

## Repository Design Principles

- Define repository interfaces in the domain layer using domain language.
- Implement repositories in the infrastructure layer with specific persistence technology.
- Return only aggregate roots from repositories, never internal entities or value objects.
- Support domain-meaningful queries, not generic CRUD operations.
- Avoid exposing query language (SQL, NoSQL) through the repository interface.
- One repository per aggregate root; do not create repositories for non-root entities.

## Query Considerations

Orthopaedic systems often require complex analytical queries (e.g., revision rates
by implant type, fracture healing times by classification). These read-heavy queries
should use CQRS read models rather than loading full aggregates through repositories.
Repositories serve the command side, providing aggregate roots for state mutations.

## Collection-Oriented vs. Persistence-Oriented

- Collection-oriented repositories mimic in-memory collections (add, remove, find).
  Suitable for contexts with straightforward persistence needs.
- Persistence-oriented repositories use explicit save operations. Suitable for
  contexts using event sourcing, where the aggregate's event stream is appended to.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
