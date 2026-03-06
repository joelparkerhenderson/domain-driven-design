# Hormone Protocol Context in the Hormone Replacement Therapy Domain

The Hormone Protocol Context is a bounded context responsible for managing HRT treatment regimen definitions, delivery method specifications, dosing algorithms, and prescription generation. This context translates clinical assessment outputs into actionable, individualized treatment protocols that specify exactly which hormones, formulations, doses, and schedules a patient will receive.

## Context Overview

This context represents the core clinical decision-making at the heart of HRT systems. It maintains the catalog of approved hormone formulations, encodes the clinical knowledge required to match patient profiles to appropriate regimens, and manages the lifecycle of treatment protocols from initial prescription through modification, suspension, and discontinuation. As a core subdomain, it receives the deepest modeling investment because it embodies the specialized pharmacological and endocrinological expertise that differentiates expert HRT management.

## Domain Model

### Aggregates

The TreatmentProtocol aggregate is the primary aggregate root, encapsulating the complete prescribed regimen for a patient. It enforces the invariant that all protocol components must be pharmacologically compatible, preventing contraindicated hormone combinations. The FormulationCatalog aggregate maintains the registry of approved formulations available for protocol construction.

### Key Entities

The TreatmentProtocol entity manages protocol lifecycle and versioning. The ProtocolComponent entity specifies a single hormone within the protocol, including its formulation, dosage, delivery method, and administration schedule. Multiple components support combination therapy regimens.

### Key Value Objects

Dosage specifies the amount, unit, and frequency of hormone administration. DeliveryMethod describes the route of administration and form factor. CyclingPattern defines the temporal pattern of hormone administration. ProtocolStatus tracks the protocol lifecycle state. FormulationSpecification details the pharmaceutical properties of an approved preparation.

## Hormone Regimen Types

### Estrogen Therapy

Estrogen-only protocols are appropriate for patients who have undergone hysterectomy. Common formulations include transdermal estradiol patches (typically 0.025 to 0.1 mg/day), oral estradiol (0.5 to 2 mg daily), topical estradiol gel, and vaginal estradiol preparations for localized symptoms. Conjugated equine estrogens are available but bioidentical estradiol is increasingly preferred based on current clinical evidence.

### Combined Estrogen-Progesterone Therapy

Patients with an intact uterus require progesterone in combination with estrogen to protect the endometrium from hyperplasia. Regimens include continuous combined (daily estrogen plus daily progesterone), sequential combined (daily estrogen with cyclic progesterone for 12-14 days per month), and long-cycle regimens. Micronized progesterone is the preferred progestogen form based on safety data from the REPLENISH and KEEPS trials.

### Testosterone Therapy

Testosterone protocols address male hypogonadism or female androgen insufficiency. Male protocols include intramuscular testosterone cypionate or enanthate (typically 100-200 mg every 1-2 weeks), transdermal testosterone patches or gel, and subcutaneous testosterone pellets. Female testosterone supplementation uses lower doses, typically 1-5 mg daily via compounded preparations, with careful monitoring.

### Combination Multi-Hormone Therapy

Some clinical presentations require supplementation of multiple hormones simultaneously. Protocols may combine estrogen, progesterone, and testosterone, or address concurrent thyroid or adrenal insufficiency. The ProtocolCompatibilityService validates these complex combinations against interaction databases and clinical guidelines.

## Protocol Lifecycle

Protocols transition through defined states: draft (created but not yet approved), pending approval (awaiting prescriber authorization), active (currently being administered), suspended (temporarily paused due to adverse events or patient request), completed (treatment course finished as planned), and discontinued (permanently stopped before planned completion). Each transition is recorded with timestamp and rationale, producing ProtocolModified or ProtocolDiscontinued domain events.

## Dosing Algorithms

The DoseCalculationService applies evidence-based algorithms to determine starting doses. Factors include patient age, body mass, baseline hormone levels, delivery method pharmacokinetics, and comorbidity adjustments. The service applies the clinical principle of starting at the lowest effective dose, with planned titration based on response. Safety constraints enforce maximum doses defined by clinical guidelines and regulatory labeling.

## Domain Events

The context publishes ProtocolPrescribed when a new protocol is activated, ProtocolModified when an active protocol is adjusted, and ProtocolDiscontinued when a protocol is permanently stopped. These events inform downstream contexts about treatment status changes.

## Context Relationships

The Hormone Protocol Context is downstream to the Clinical Assessment Context (customer-supplier), receiving assessment data that drives protocol selection. It is upstream to the Lab Monitoring Context, supplying monitoring requirements. It receives optimization recommendations from the Treatment Optimization Context. It maintains an anti-corruption layer against external pharmacy systems for prescription fulfillment.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- The Endocrine Society. Clinical Practice Guidelines for Testosterone Therapy, 2018.
- North American Menopause Society. "The 2022 Hormone Therapy Position Statement." Menopause, 2022.
- Stuenkel, C.A., et al. "Treatment of Symptoms of the Menopause." JCEM, 2015.
