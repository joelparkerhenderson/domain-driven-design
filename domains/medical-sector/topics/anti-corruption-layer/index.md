# Anti-Corruption Layer in Medical Practice

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context's domain model from the concepts, data formats, and assumptions of external systems. In medical practice, ACLs are essential because the domain integrates with numerous external systems -- insurance payers, reference laboratories, drug databases, imaging archives, and health information exchanges -- each with its own data models and communication protocols.

## Why ACLs Are Critical in Medicine

Medical systems interact with many external parties that impose their own standards, formats, and semantics:

- Insurance payers require EDI transaction sets (837, 835, 270/271) with specific field mappings
- Reference laboratories send results in HL7 v2 message formats with lab-specific coding
- PACS and imaging archives use DICOM protocols with modality-specific metadata
- Drug interaction databases expose proprietary APIs with their own medication identifiers
- Health information exchanges use C-CDA documents or FHIR bundles with varying profiles

Without an ACL, these external models leak into the core domain, creating:

- Tight coupling to external system changes
- Confusion between internal domain concepts and external data representations
- Difficulty maintaining a clean ubiquitous language
- Fragile integrations that break when external systems update their APIs

## ACL Examples in Medical Practice

### External Lab Systems ACL

External reference laboratories send results as HL7 v2 ORU messages. The ACL translates:

- HL7 OBX segments into internal LabResult value objects
- External test codes (lab-specific) into internal LOINC-based test identifiers
- HL7 abnormal flags (H, L, A, AA) into internal AbnormalityLevel value objects
- HL7 patient identifiers into internal MRN references

### Insurance Payer ACL

Insurance payers communicate via EDI transactions. The ACL translates:

- Internal Claim aggregates into 837P/837I EDI transaction sets for submission
- 835 remittance advice transactions into internal AdjudicationResult value objects
- 270/271 eligibility inquiry/response pairs into internal CoverageStatus value objects
- Payer-specific denial codes into internal DenialReason enumerations

### Drug Database ACL

External drug interaction and formulary databases use proprietary data models. The ACL translates:

- External drug identifiers into internal RxNorm or NDC codes
- Proprietary interaction severity levels into internal InteractionSeverity value objects
- External formulary tier codes into internal FormularyTier value objects

### DICOM/PACS ACL

Picture Archiving and Communication Systems use DICOM protocols. The ACL translates:

- DICOM study metadata into internal ImagingStudy entities
- DICOM Series and Instance identifiers into internal image references
- DICOM modality codes into internal Modality value objects
- DICOM patient demographics into verified MRN cross-references

## Design Principles

- **Translate at the boundary**: All external data is converted into internal domain objects before entering the bounded context
- **No external types inside**: Internal domain code never references external data structures
- **Bidirectional translation**: ACLs handle both inbound (receiving) and outbound (sending) translations
- **Isolated change impact**: When an external system changes its API, only the ACL needs updating
- **Testable in isolation**: ACL translation logic can be unit-tested with sample external data

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- ACLs protect bounded context boundaries
- [Context Map](../context-map/) -- ACLs appear as integration patterns on the context map
- [Integration Patterns](../integration-patterns/) -- ACL is one of several integration patterns
- [Value Objects](../value-objects/) -- ACLs translate external data into internal value objects
- [Medical Standards](../medical-standards/) -- Standards like HL7, FHIR, and DICOM are what ACLs translate
- [Regulatory Compliance](../regulatory-compliance/) -- ACLs must preserve compliance-relevant data during translation

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 8. Wrox, 2015.
- Benson, Tim & Grieve, Grahame. "Principles of Health Interoperability: FHIR, HL7 and SNOMED CT." Springer, 2021.
- HL7 International. "HL7 v2 Message Specifications." https://www.hl7.org/implement/standards/
