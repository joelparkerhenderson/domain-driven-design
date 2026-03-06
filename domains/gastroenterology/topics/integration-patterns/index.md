# Integration Patterns in Gastroenterology

## Overview

Integration patterns define how bounded contexts within the gastroenterology domain
communicate with each other and with external systems. These patterns are drawn from
DDD strategic design and enterprise integration practices, adapted to the specific
needs of clinical gastroenterology where data integrity, timeliness, and regulatory
compliance are essential.

## Internal Integration Patterns

### Shared Kernel

A minimal shared kernel exists across all six bounded contexts. The shared kernel
contains only the most fundamental identifiers and types that must be consistent
everywhere:

- PatientId: unique patient identifier used across all contexts.
- ProviderId: unique clinician identifier.
- EncounterId: unique clinical encounter identifier.
- DiagnosisCode (ICD-10): standardized diagnosis coding.

The shared kernel is deliberately kept small. Adding to the shared kernel requires
agreement from all context teams because changes affect every bounded context. Domain
concepts that appear similar across contexts (e.g., "finding," "diagnosis," "treatment")
are intentionally excluded from the shared kernel because they have context-specific
semantics.

### Published Language

Bounded contexts communicate through published language contracts that define the
structure and semantics of shared data. Each publishing context defines its output
contracts; consuming contexts adapt to these contracts.

The Endoscopic Procedures Context publishes procedure reports using a defined procedure
report schema. The Inflammatory Disease Context publishes disease activity summaries
using a defined activity report schema. These schemas are versioned and evolved with
backward compatibility.

### Customer-Supplier

Several context relationships follow the customer-supplier pattern where a downstream
context depends on data provided by an upstream context:

- Clinical Assessment (supplier) provides referrals and lab data to all specialized
  contexts (customers).
- Endoscopic Procedures (supplier) provides findings and pathology results to
  Inflammatory Disease (customer) and Hepatology (customer).
- Hepatology (supplier) provides liver disease status and dietary requirements to
  Nutrition Management (customer).

In customer-supplier relationships, the downstream context can negotiate with the
upstream context about the data it needs, but the upstream context controls the
delivery format and timing.

### Partnership

The Endoscopic Procedures Context and the Inflammatory Disease Context have a
partnership relationship. Both contexts must collaborate closely because endoscopic
surveillance is integral to IBD management, and IBD disease activity directly influences
endoscopic scheduling and interpretation. Both teams coordinate their models and
integration contracts jointly.

### Conformist

The Nutrition Management Context acts as a conformist to the Clinical Assessment
Context for laboratory data. Rather than defining its own lab result model, it adopts
the Clinical Assessment Context's LabResult value object directly. This is appropriate
because lab values are objective measurements that do not require reinterpretation for
nutritional assessment purposes.

## External System Integration

### Electronic Health Record (EHR) Integration

The EHR system is the primary external system. Integration occurs through anti-corruption
layers at each bounded context that needs EHR data. The Clinical Assessment Context
has the most extensive EHR integration, receiving patient demographics, medication lists,
and problem lists. Each context translates EHR data structures (typically HL7 FHIR
resources) into its own domain model.

### Laboratory Information System (LIS) Integration

Lab results flow from the LIS into the Clinical Assessment Context, which then
distributes relevant results to downstream contexts through domain events. The
anti-corruption layer translates LOINC-coded results into domain-specific value
objects (LabResult, LiverFunctionPanel, InflammatoryMarkerPanel).

### Pharmacy Integration

The Inflammatory Disease Context and the Hepatology Context integrate with pharmacy
systems for medication management. The anti-corruption layer translates between
pharmacy NDC codes and the domain's therapy classification. Prescription orders flow
outbound; dispensing confirmations flow inbound.

### Pathology Integration

Pathology results flow from the anatomic pathology system into the Endoscopic
Procedures Context. The anti-corruption layer translates synoptic pathology reports
into PathologyResult value objects. These results are then distributed to disease
management contexts through domain events.

### Radiology Integration

Imaging orders flow from the Clinical Assessment Context to the radiology system.
Imaging results and reports flow back through the anti-corruption layer, translated
into ImagingResult value objects.

## Integration Principles

1. No bounded context directly accesses another context's data store.
2. External system models never leak into domain model code.
3. Integration contracts are versioned and evolved with backward compatibility.
4. All data crossing a context boundary passes through explicit translation.
5. Asynchronous communication is preferred over synchronous for cross-context data flow.
6. Integration failures are logged and retried without data loss.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Maintaining Model Integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3: Context Maps.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7: Context Mapping Patterns.
- Hohpe, G. & Woolf, B. (2003). *Enterprise Integration Patterns*. Addison-Wesley.
- HL7 FHIR R4 specification for healthcare data interoperability standards.
