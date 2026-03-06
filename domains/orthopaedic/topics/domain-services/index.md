# Domain Services in the Orthopaedic Domain

## Overview

A domain service is a stateless operation that encapsulates domain logic that does
not naturally belong to any single entity or value object. Domain services express
significant domain concepts that span multiple aggregates or require coordination
between domain objects. In the orthopaedic domain, domain services capture clinical
decision-making processes and cross-aggregate workflows.

## Purpose

Some orthopaedic business operations involve multiple aggregates or require access
to external clinical knowledge that no single aggregate owns. For example, determining
implant compatibility requires knowledge of the patient's anatomy, the implant catalog,
and the planned surgical approach. No single aggregate owns all this information, so
a domain service coordinates the operation.

## Key Domain Services

### Clinical Assessment Services

**ClassificationService**: Applies standardized classification systems to clinical
findings. Given examination data and imaging results, this service determines the
appropriate classification (e.g., Kellgren-Lawrence grade for osteoarthritis,
Garden classification for femoral neck fractures). The service encapsulates the
classification algorithms without storing state.

**ClinicalPriorityService**: Assesses the urgency of a clinical case based on
diagnosis, patient factors, and clinical guidelines. Returns a priority level that
influences scheduling and resource allocation.

### Surgical Planning Services

**ImplantCompatibilityService**: Validates that a set of selected implant components
are compatible with each other and with the patient's anatomy. Checks component
sizes, material compatibility, and manufacturer combination rules. Returns a
compatibility assessment with any identified conflicts.

**TemplatingService**: Performs digital templating calculations using imaging
measurements and implant specifications. Determines optimal implant size and
positioning based on anatomical landmarks and the planned surgical approach.

**OperativeRiskService**: Calculates the operative risk score based on patient
comorbidities, BMI, age, ASA grade, and procedure complexity. Uses validated
scoring systems to produce a quantified risk assessment.

### Joint Replacement Services

**RevisionAssessmentService**: Evaluates whether an arthroplasty case warrants
revision based on outcome score trends, imaging findings, and clinical symptoms.
Applies clinical criteria to produce a recommendation for continued surveillance,
further investigation, or revision planning.

**RegistrySubmissionService**: Prepares arthroplasty case data for submission to
national joint registries. Validates data completeness, maps internal representations
to registry schemas, and generates submission records.

### Sports Medicine Services

**ReturnToPlayEvaluationService**: Assesses whether an athlete meets the criteria
for return to sport. Evaluates functional test results, imaging findings, and
time-since-injury against protocol-defined thresholds. Returns a clearance
recommendation with any unmet criteria.

**InjuryRecurrenceRiskService**: Calculates the risk of injury recurrence based on
injury type, sport demands, rehabilitation progress, and historical patterns.
Informs return-to-play timing decisions.

### Fracture Management Services

**FractureClassificationService**: Applies the AO/OTA classification system to
imaging data. Takes bone identification, fracture location, and morphology
descriptors as input and produces the standardized classification code.

**HealingProgressService**: Evaluates fracture healing progress by comparing
sequential imaging assessments against expected healing timelines for the
fracture type and fixation method. Identifies delayed union or nonunion risk.

### Rehabilitation Services

**ProtocolSelectionService**: Determines the appropriate rehabilitation protocol
based on the procedure performed, patient factors, surgeon preferences, and
weight-bearing restrictions. Returns the selected protocol with phase definitions
and milestone targets.

**FunctionalProgressService**: Evaluates a patient's rehabilitation progress by
comparing current functional measurements against protocol-defined targets for the
current phase. Identifies patients ahead of or behind expected progress.

## Domain Service Design Principles

- Domain services are stateless: they do not hold data between invocations.
- Name services using the ubiquitous language (e.g., ImplantCompatibilityService,
  not CompatibilityChecker).
- Define service interfaces in the domain layer.
- Domain services may use repositories to retrieve aggregates needed for their logic.
- Avoid creating services for logic that belongs in an entity or value object.
- A service should represent a complete domain operation, not a technical utility.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
