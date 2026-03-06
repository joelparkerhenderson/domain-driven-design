# Medical Records Release Permission Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to medical records release permission systems, encompassing authorization management, patient consent workflows, request processing, disclosure tracking, regulatory compliance, and audit and accountability.

## Bounded Contexts

1. Authorization Management Context - authorization form creation, scope definition (records types, date ranges, purposes), granular permission specification, revocation handling, expiration enforcement
2. Patient Consent Context - informed consent collection, consent capacity verification, minor and guardian consent, consent preferences management, patient identity verification
3. Request Processing Context - release request intake, requestor identity verification, request validation against authorization scope, record retrieval coordination, fulfillment tracking
4. Disclosure Tracking Context - disclosure event logging, accounting of disclosures (HIPAA requirement), recipient tracking, volume and frequency monitoring, disclosure history reporting
5. Regulatory Compliance Context - HIPAA Privacy Rule enforcement, state privacy law application, minimum necessary standard evaluation, protected health information (PHI) category handling, breach notification protocols

## Domain Principles

- Model using shared ubiquitous language between health information management professionals, privacy officers, compliance teams, patients, and system developers.
- Group the system into bounded contexts reflecting the operational stages of medical records release from authorization through audit.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow HIPAA Privacy Rule, HITECH Act, and state-specific health privacy law terminology and requirements.
- Maintain patient autonomy, minimum necessary disclosure, and privacy by design as cross-cutting concerns.

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
- authorization-management-context
- patient-consent-context
- request-processing-context
- disclosure-tracking-context
- regulatory-compliance-context
- event-driven-architecture
- integration-patterns
- privacy-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- U.S. Department of Health and Human Services. HIPAA Privacy Rule (45 CFR Parts 160 and 164).
- AHIMA. Health Information Management: Concepts, Principles, and Practice. AHIMA Press, 2020.
