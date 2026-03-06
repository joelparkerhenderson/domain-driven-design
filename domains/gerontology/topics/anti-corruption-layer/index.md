# Anti-Corruption Layer in the Gerontology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the models, terminology, and assumptions of external systems or other bounded contexts. Evans (2003) introduces the ACL as a defensive pattern that prevents model corruption when integrating with systems whose models differ from the domain's ubiquitous language. In the gerontology domain, ACLs are essential because geriatric care systems must integrate with hospital EHRs, pharmacy systems, insurance platforms, and community service directories, each with its own model of patients, encounters, and services.

## Why ACLs Matter in Gerontology

Geriatric care operates at the intersection of multiple healthcare systems, each designed with different assumptions. A hospital EHR models a patient encounter as a discrete admission-discharge event, while the gerontology domain models care as a longitudinal episode spanning months or years. A pharmacy system models medications by NDC codes and formulary tiers, while the gerontology domain models medications in terms of appropriateness criteria (Beers list), deprescribing potential, and adherence patterns.

Without ACLs, these external models would leak into the gerontology domain, introducing concepts and structures that do not reflect geriatric care reality. The ACL acts as a translator, converting external representations into the domain's ubiquitous language and vice versa.

## ACL Between Geriatric Assessment and Hospital EHR

The hospital EHR contains patient demographics, problem lists, and lab results that the Geriatric Assessment Context needs. However, the EHR's model of a "problem" (ICD-10 coded diagnosis) differs from the gerontology domain's model of a "geriatric syndrome" (a multifactorial clinical condition). The ACL translates ICD-10 coded problems into geriatric syndrome classifications, maps EHR encounter data into the domain's longitudinal care episode model, and converts lab result formats into the value objects used by frailty and nutritional assessment aggregates.

## ACL Between Medication Management and Pharmacy Systems

Pharmacy systems use NDC codes, therapeutic classes, and formulary status to represent medications. The Medication Management Context models medications in terms of Beers criteria categories, deprescribing priority, and renal dose adjustment requirements. The ACL translates between these representations, mapping NDC codes to Beers criteria classifications, converting formulary restrictions into the domain's medication appropriateness model, and translating pharmacy refill data into medication adherence records.

## ACL Between Care Coordination and Insurance Systems

Insurance and payer systems model services in terms of CPT codes, authorization numbers, and benefit coverage. The Care Coordination Context models services as care plan components with clinical goals, provider assignments, and patient preferences. The ACL translates authorization decisions into care plan constraints, maps CPT-coded services into the domain's care activity model, and converts benefit coverage information into resource availability for community service referrals.

## ACL Between Cognitive Health and Neuropsychology Systems

Specialized neuropsychology testing systems produce detailed cognitive profiles using domain-specific scoring conventions. The Cognitive Health Context needs to consume these results while maintaining its own model of cognitive status. The ACL translates raw neuropsychological test data into the domain's CognitiveScreeningResult value object, maps external dementia severity classifications into the domain's DementiaStage value object, and converts longitudinal test data into the cognitive trend analysis format.

## ACL Between End of Life Planning and Legal Document Systems

Legal document management systems store advance directives, POLST forms, and power-of-attorney designations using legal terminology and document structures. The End of Life Planning Context models these as domain objects with clinical significance. The ACL translates legal document metadata into AdvanceDirective entities, maps POLST form fields into the domain's treatment preference model, and converts legal representative designations into surrogate decision-maker roles.

## Implementation Considerations

ACLs in the gerontology domain typically implement the adapter pattern, providing a translation layer between external system interfaces and internal domain models. The translation logic should be explicit, testable, and owned by the bounded context team that depends on the external data. When external systems change their models, only the ACL needs updating, protecting the core domain model from disruption.

Vernon (2013) recommends that ACLs use domain events or application services as integration points rather than directly coupling to external system APIs. This approach provides additional insulation and allows for asynchronous integration where appropriate.

## Bidirectional Translation

ACLs in gerontology often need to translate in both directions. When the gerontology domain publishes assessment results to a hospital EHR, the ACL must translate domain concepts into EHR-compatible formats (such as HL7 FHIR resources). When receiving data from external systems, the ACL translates incoming data into domain objects. This bidirectional translation ensures that the domain model remains pure while supporting necessary data exchange.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- HL7 International. (2019). FHIR (Fast Healthcare Interoperability Resources) Specification.
