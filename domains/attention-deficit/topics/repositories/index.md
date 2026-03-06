# Repositories: Attention-Deficit Domain

Repositories provide abstractions for storing and retrieving aggregate roots in the ADHD management domain. They encapsulate persistence concerns behind domain-oriented interfaces, allowing the domain model to remain independent of storage technology and infrastructure details.

## Purpose

ADHD management systems must persist clinical assessments, treatment plans, behavioral monitoring data, educational accommodations, medication records, and outcome measurements over extended periods. Repositories ensure that the domain model interacts with persistence through interfaces expressed in ubiquitous language terms, not database-specific operations. A clinician retrieves a patient's assessment history, not rows from a database table.

## Repository Design Principles

### Aggregate Root Access Only

Repositories provide access exclusively through aggregate roots. There is no repository for individual Rating Scale Scores or Side Effect Reports. These child entities and value objects are accessed through their parent aggregate. The Patient Assessment Repository retrieves complete Patient Assessment aggregates, including all associated Assessment Sessions, Rating Scale Scores, and Diagnostic Impressions.

### Domain-Oriented Query Methods

Repository interfaces express queries in domain language:

- `findActiveAssessmentsByClinicianAndDateRange` rather than generic SQL-like queries.
- `findPatientsWithPendingTitrationReview` rather than filtering by raw status codes.
- `findAccommodationPlansRequiringAnnualReview` rather than date arithmetic on generic records.

### Collection-Oriented vs. Persistence-Oriented

Evans (2003) distinguishes between collection-oriented repositories (which simulate an in-memory collection) and persistence-oriented repositories (which make persistence explicit). The ADHD management domain favors collection-oriented semantics: the Treatment Plan Repository behaves as if all treatment plans are in memory, hiding the persistence mechanism entirely.

## Clinical Assessment Context Repository

### PatientAssessmentRepository

Provides access to Patient Assessment aggregates. Core operations:

- Retrieve a patient's complete assessment history by patient identifier.
- Find assessments by diagnostic outcome (specific ADHD presentation type, comorbidity pattern).
- Find assessments pending clinician review or signature.
- Find assessments by rating scale results exceeding clinical significance thresholds.
- Store new or updated Patient Assessment aggregates.

## Treatment Management Context Repository

### TreatmentPlanRepository

Provides access to Treatment Plan aggregates. Core operations:

- Retrieve active treatment plans for a specific patient.
- Find treatment plans by modality combination (e.g., all plans including both behavioral therapy and parent training).
- Find treatment plans approaching scheduled review dates.
- Find treatment plans with treatment episodes in specific lifecycle states (active, paused, concluded).
- Store new or updated Treatment Plan aggregates.

## Behavioral Monitoring Context Repository

### BehaviorRecordRepository

Provides access to Behavior Record aggregates. Core operations:

- Retrieve the current behavior record for a specific individual.
- Find behavior records with entries showing significant symptom frequency changes over a specified period.
- Find behavior records by setting type (home, school, clinical) and date range.
- Find behavior records with executive function assessment results below threshold.
- Store new or updated Behavior Record aggregates.

## Educational Accommodation Context Repository

### AccommodationPlanRepository

Provides access to Accommodation Plan aggregates (IEP and 504 plans). Core operations:

- Retrieve active accommodation plans for a specific student.
- Find plans by accommodation type (extended time, preferential seating, organizational support).
- Find plans requiring annual review within a specified timeframe.
- Find plans by eligibility category and school placement.
- Store new or updated Accommodation Plan aggregates.

## Medication Management Context Repository

### MedicationRegimenRepository

Provides access to Medication Regimen aggregates. Core operations:

- Retrieve the current medication regimen for a specific patient.
- Find regimens with active titration schedules requiring follow-up.
- Find regimens by medication class (stimulant, non-stimulant) and response status.
- Find regimens with reported side effects exceeding a severity threshold.
- Store new or updated Medication Regimen aggregates.

## Outcomes Tracking Context Repository

### OutcomeMeasureRepository

Provides access to Outcome Measure aggregates. Core operations:

- Retrieve outcome measurement history for a specific patient.
- Find outcome measures by instrument type and measurement timepoint.
- Find patients classified as non-responders to current treatment.
- Find outcome measures showing clinically significant improvement or decline.
- Store new or updated Outcome Measure aggregates.

## Cross-Cutting Repository Concerns

**No cross-context repositories.** Each repository belongs to exactly one bounded context. There is no repository that spans Clinical Assessment and Treatment Management. Cross-context data access occurs through domain events and integration interfaces.

**Repository interfaces belong to the domain layer.** The interface definition uses domain terminology and lives in the domain model. The implementation, which handles actual database interaction, lives in the infrastructure layer.

**Testing through repository abstractions.** Domain logic is tested using in-memory repository implementations, isolating business rules from database concerns. This is especially important for ADHD management systems where clinical logic must be validated independently of persistence technology.

**Audit trail support.** ADHD management requires comprehensive audit trails for clinical, legal, and regulatory compliance. Repositories support event sourcing or change tracking patterns to maintain complete modification histories for all aggregate roots.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
