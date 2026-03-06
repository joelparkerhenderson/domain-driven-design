# Anti-Corruption Layer for Mast Cell Activation Syndrome

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, models, and terminology. In the MCAS domain, ACLs are essential because the system must integrate with external healthcare systems including electronic health records, laboratory information systems, pharmacy systems, and insurance platforms, each with their own data models and terminology.

Without ACLs, external system concepts would leak into the MCAS domain model, creating tight coupling and fragile integrations. The ACL translates between external representations and the internal ubiquitous language, ensuring the domain model remains pure and clinically meaningful.

## ACL for Laboratory Information Systems

The Diagnostic Assessment Context requires integration with laboratory information systems (LIS) that report results for tryptase, histamine metabolites, prostaglandin D2, and other mediator measurements. External LIS use standardized coding systems such as LOINC for test identification and report results in formats that do not align with the MCAS domain model.

The ACL translates LIS result messages into the domain's Mediator Level value objects. It maps LOINC codes to domain-specific test identifiers, converts units of measurement to the domain's standard units, and transforms result status codes into domain-relevant states. The ACL also handles reference range translation, since MCAS-specific clinical thresholds may differ from standard laboratory reference ranges.

For example, a LIS may report serum tryptase as a numeric value with a standard reference range. The ACL translates this into a Tryptase Level value object that includes the measured value, the patient's personal baseline, and an interpretation of whether the result meets the consensus criteria threshold for significant elevation.

## ACL for Electronic Health Records

The Specialist Coordination Context integrates with external EHR systems to access patient demographics, provider directories, and clinical documentation. EHR systems use data models such as HL7 FHIR resources that differ significantly from the MCAS domain model.

The ACL translates FHIR Patient resources into the domain's Patient entity, mapping relevant clinical attributes while discarding EHR-specific administrative data. Provider information from FHIR Practitioner resources is translated into the domain's Care Team Member model. Clinical documents are mapped to the domain's Shared Care Plan format.

The ACL also handles the reverse translation, converting domain events and care plan updates into FHIR-compatible resources for transmission to the EHR. This bidirectional translation ensures the MCAS domain is not contaminated by FHIR-specific concepts.

## ACL for Pharmacy Systems

The Medication Protocol Context integrates with pharmacy systems for prescription processing, formulary checking, and compounding requests. Pharmacy systems use medication databases with identifiers such as NDC codes and RxNorm concepts that differ from the domain's medication model.

The ACL translates between pharmacy formulary entries and the domain's Medication entity. It maps NDC codes to domain medication identifiers, translates dosage forms and strengths, and converts dispensing instructions into the domain's Titration Schedule format. For compounded medications, the ACL generates compounding specifications from the domain's formulation model.

## ACL for Insurance and Billing Systems

Although billing is not a core concern of the MCAS domain, integration with insurance systems is necessary for prior authorization of specialty medications and diagnostic procedures. The ACL translates between the domain's Medication Regimen and Diagnostic Workup models and the insurance system's authorization request formats, typically using X12 or FHIR coverage resources.

## Design Principles for MCAS ACLs

Each ACL in the MCAS domain follows consistent design principles. The ACL is owned by the consuming bounded context, not by the external system. Translation logic is centralized in the ACL rather than scattered throughout the domain model. The ACL handles data format conversion, terminology mapping, and protocol adaptation. Error handling within the ACL translates external system errors into domain-meaningful exceptions.

The ACL is the only point of contact between the internal domain model and external systems. No external data model classes, identifiers, or terminologies are allowed to pass through the ACL into the domain layer.

## Testing ACLs

ACLs require thorough testing because they handle the inherently messy work of external integration. Tests verify that external data formats are correctly translated into domain objects, that edge cases in external data are handled gracefully, and that the domain model is never exposed to external system concepts.

## Evolution of ACLs

As external systems evolve their APIs and data models, ACLs absorb the impact of these changes. The domain model remains stable while ACL translation logic is updated to accommodate new versions of external interfaces. This insulation is particularly valuable in healthcare, where standards such as HL7 FHIR undergo regular revisions.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on anti-corruption layers and integration.
