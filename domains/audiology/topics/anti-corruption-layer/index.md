# Anti-Corruption Layer in the Audiology Domain

## Overview

The Anti-Corruption Layer (ACL) is a translation boundary that protects a bounded context from the concepts, models, and language of external systems. In the audiology domain, ACLs are essential at the boundaries where the domain model interfaces with hearing device manufacturer systems, electronic health record (EHR) platforms, audiometer data transfer protocols, and insurance billing systems. The ACL ensures that external system complexity does not leak into and corrupt the carefully designed audiology domain model.

## Purpose

External systems used in audiology practice were not designed with the audiology domain model in mind. Hearing aid manufacturer software uses proprietary data formats and device-specific terminology. EHR systems impose their own models of clinical encounters that may not align with audiologic workflow. Insurance billing systems use procedure codes and claim structures that reflect reimbursement logic rather than clinical logic. The ACL translates between these foreign models and the audiology domain model, preserving the integrity of the ubiquitous language within each bounded context.

## ACL in the Device Management Context

The Device Management Context maintains the most critical ACL in the audiology domain. Hearing aid manufacturers provide programming interfaces (such as NOAH and manufacturer-specific fitting software) with their own data models, terminology, and constraints. A manufacturer's concept of a "program" or "channel" may not align with the domain model's representation of these concepts.

The ACL translates manufacturer-specific fitting parameters into the domain model's fitting prescription value objects. When the domain model requests a fitting adjustment, the ACL translates this request into the manufacturer's proprietary format. When the manufacturer system returns verification data, the ACL translates it into domain model value objects that the Device Management Context understands.

This translation preserves the domain model's independence from any single manufacturer. If the practice switches manufacturers or adds a new product line, only the ACL adapter changes. The core device management model remains stable.

## ACL in the Clinical Documentation Context

The Clinical Documentation Context maintains an ACL for electronic health record systems. EHR platforms like Epic, Cerner, or Athenahealth impose their own encounter models, note templates, and data exchange formats (typically HL7 FHIR or HL7 v2). The ACL translates audiology-specific clinical documents (audiograms, fitting reports, vestibular evaluations) into EHR-compatible formats for storage and retrieval.

The ACL also handles the reverse translation. When the Documentation Context retrieves patient demographic data or medical history from the EHR, the ACL filters and translates this information into the domain model's representation of patient context, discarding EHR-specific metadata that has no meaning in the audiology domain.

## ACL in the Hearing Assessment Context

The Hearing Assessment Context maintains an ACL for audiometer systems and data transfer protocols. Audiometers from different manufacturers (Interacoustics, Grason-Stadler, Otometrics) produce data in various formats. The ACL normalizes these proprietary formats into the domain model's standardized audiometric data structures.

The ACL also enforces calibration constraints. When receiving data from an audiometer, the ACL validates that the equipment is within calibration specifications (per ANSI S3.6) before allowing the data to enter the domain model. This prevents uncalibrated or suspect measurements from corrupting diagnostic records.

## ACL for Insurance and Billing Systems

Multiple bounded contexts interface with insurance and billing systems through a shared ACL. Insurance systems use CPT codes, ICD-10 diagnostic codes, and claim structures that represent clinical activities in reimbursement terms rather than clinical terms. The ACL translates clinical events (a completed audiometric evaluation, a device fitting session) into billing-compatible procedure codes while keeping billing logic entirely outside the clinical domain model.

## Design Principles

ACLs in the audiology domain follow several design principles. The ACL is always owned by the downstream (consuming) bounded context, never by the external system. The ACL translates bidirectionally, handling both incoming data from external systems and outgoing requests to external systems. The ACL validates incoming data against domain model invariants, rejecting data that would violate domain rules. The ACL is the only point of contact with the external system; no other part of the bounded context communicates directly with external systems.

## Implementation Patterns

ACLs in the audiology domain use adapter and facade patterns. An adapter wraps the external system's interface and presents it in terms the domain model understands. A facade simplifies complex external system interactions into domain-meaningful operations. Translation logic lives in dedicated mapping components that convert between external data transfer objects and domain value objects or entities.

## Failure Handling

The ACL must handle external system failures gracefully. If a manufacturer's programming interface is unavailable, the ACL reports this as a domain-meaningful event rather than propagating infrastructure errors. If an EHR system returns incomplete data, the ACL identifies missing elements and either requests them or flags the gaps for clinical review.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. Chapter 14 on anti-corruption layers.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley. Chapter 3 on integrating bounded contexts.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media. Chapter 6 on anti-corruption layer pattern.
- HIMSS. Interoperability in Healthcare. Health Information Management Systems Society.
