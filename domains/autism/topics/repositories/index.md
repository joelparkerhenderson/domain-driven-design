# Repositories: Autism Domain

Repositories provide abstractions for storing and retrieving aggregate roots, encapsulating persistence logic and presenting a collection-like interface to the domain model within the autism spectrum management domain.

## Purpose

In the autism domain, aggregates must be persisted and retrieved without the domain model depending on specific storage technologies. Repositories define the contract for how aggregates are saved, found, and removed, keeping the domain model pure and independent of infrastructure concerns. Each aggregate root has its own repository, and the repository interface is expressed in terms of the ubiquitous language.

## Diagnostic Assessment Context Repositories

### DiagnosticEvaluationRepository

Manages persistence of DiagnosticEvaluation aggregates. Key operations:

- findById(evaluationId): Retrieve a complete diagnostic evaluation by its identifier.
- findByIndividual(individualId): Retrieve all diagnostic evaluations for a specific individual, ordered by evaluation date.
- findByStatus(status): Retrieve evaluations filtered by lifecycle status (Initiated, Screening Complete, Instruments Administered, Determination Made, Report Finalized).
- findPendingEvaluations(): Retrieve evaluations awaiting clinician review or determination.
- save(evaluation): Persist a new or updated diagnostic evaluation.

### InstrumentAdministrationRepository

Manages persistence of InstrumentAdministration aggregates. Key operations:

- findById(administrationId): Retrieve a specific instrument administration.
- findByEvaluation(evaluationId): Retrieve all instrument administrations within a diagnostic evaluation.
- findByInstrumentType(instrumentType): Retrieve administrations filtered by instrument (ADOS-2, ADI-R, Bayley-4).
- save(administration): Persist a new or updated instrument administration.

## Behavioral Intervention Context Repositories

### InterventionPlanRepository

Manages persistence of InterventionPlan aggregates. Key operations:

- findById(planId): Retrieve a specific behavior intervention plan.
- findActiveByIndividual(individualId): Retrieve the currently active BIP for an individual.
- findBySupervising BCBA(bcbaId): Retrieve all plans supervised by a specific BCBA.
- findRequiringReview(reviewDate): Retrieve plans due for periodic review.
- save(plan): Persist a new or updated intervention plan.

### SkillAcquisitionProgramRepository

Manages persistence of SkillAcquisitionProgram aggregates. Key operations:

- findById(programId): Retrieve a specific skill acquisition program.
- findActiveByIndividual(individualId): Retrieve all active programs for an individual.
- findByDomain(skillDomain): Retrieve programs filtered by developmental domain (communication, social, adaptive, academic, motor).
- findApproachingMastery(threshold): Retrieve programs where recent data approaches mastery criteria.
- save(program): Persist a new or updated skill acquisition program.

### TherapySessionRepository

Manages persistence of TherapySession aggregates. Key operations:

- findById(sessionId): Retrieve a specific therapy session record.
- findByIndividualAndDateRange(individualId, startDate, endDate): Retrieve sessions within a date range.
- findByTherapist(therapistId, date): Retrieve all sessions for a therapist on a given date.
- save(session): Persist a new or updated therapy session.

## Sensory Processing Context Repositories

### SensoryProfileRepository

Manages persistence of SensoryProfile aggregates. Key operations:

- findById(profileId): Retrieve a specific sensory profile.
- findLatestByIndividual(individualId): Retrieve the most recent sensory profile for an individual.
- findByQuadrantConcern(quadrant, threshold): Retrieve profiles where a specific sensory quadrant exceeds a concern threshold.
- save(profile): Persist a new or updated sensory profile.

### SensoryDietRepository

Manages persistence of SensoryDiet aggregates. Key operations:

- findById(dietId): Retrieve a specific sensory diet plan.
- findActiveByIndividual(individualId): Retrieve the currently active sensory diet for an individual.
- findByTargetSensorySystem(sensorySystem): Retrieve diets targeting a specific sensory system.
- save(diet): Persist a new or updated sensory diet.

## Communication Support Context Repositories

### CommunicationPlanRepository

Manages persistence of CommunicationPlan aggregates. Key operations:

- findById(planId): Retrieve a specific communication plan.
- findActiveByIndividual(individualId): Retrieve the active communication plan for an individual.
- findByModality(communicationModality): Retrieve plans filtered by primary communication modality (verbal, PECS, AAC device).
- save(plan): Persist a new or updated communication plan.

### AACConfigurationRepository

Manages persistence of AACConfiguration aggregates. Key operations:

- findById(configId): Retrieve a specific AAC configuration.
- findByIndividual(individualId): Retrieve all AAC configurations (current and historical) for an individual.
- findActiveByDeviceType(deviceType): Retrieve active configurations filtered by device category.
- save(config): Persist a new or updated AAC configuration.

## Educational Planning Context Repositories

### IEPDocumentRepository

Manages persistence of IEPDocument aggregates. Key operations:

- findById(iepId): Retrieve a specific IEP document.
- findCurrentByIndividual(individualId): Retrieve the currently active IEP for an individual.
- findBySchool(schoolId, academicYear): Retrieve all IEPs for a specific school and academic year.
- findDueForAnnualReview(dateRange): Retrieve IEPs with annual reviews due within a date range.
- save(iep): Persist a new or updated IEP document.

### TransitionPlanRepository

Manages persistence of TransitionPlan aggregates. Key operations:

- findById(planId): Retrieve a specific transition plan.
- findByIndividual(individualId): Retrieve the transition plan for an individual.
- findByPostSecondaryGoalArea(goalArea): Retrieve plans filtered by post-secondary goal area (education, employment, independent living).
- save(plan): Persist a new or updated transition plan.

## Outcomes Tracking Context Repositories

### OutcomeMeasureRepository

Manages persistence of OutcomeMeasure aggregates. Key operations:

- findById(measureId): Retrieve a specific outcome measure.
- findByIndividualAndDomain(individualId, outcomeDomain): Retrieve outcome measures filtered by individual and domain.
- findByMeasurementPeriod(periodId): Retrieve all measures for a specific measurement period.
- save(measure): Persist a new or updated outcome measure.

### ProgressReportRepository

Manages persistence of ProgressReport aggregates. Key operations:

- findById(reportId): Retrieve a specific progress report.
- findByIndividualAndDateRange(individualId, startDate, endDate): Retrieve progress reports within a date range.
- findLatestByIndividual(individualId): Retrieve the most recent progress report.
- save(report): Persist a new or updated progress report.

## Repository Design Principles

- Aggregate root access only: Repositories exist only for aggregate roots. Internal aggregate objects are accessed through their root.
- Collection semantics: Repositories present a collection-like interface (add, find, remove) rather than exposing storage implementation details.
- Domain language: Repository method names use the ubiquitous language, not database terminology.
- Infrastructure independence: Repository interfaces are defined in the domain layer. Implementations reside in the infrastructure layer.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley. Chapter 6 on repositories.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 12 on repository implementation.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 9 on persistence and repositories.
