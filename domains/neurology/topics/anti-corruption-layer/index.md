# Anti-Corruption Layer - Neurology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from being corrupted by external system concepts, data models, or terminology. In the neurology domain, anti-corruption layers are essential at integration points where external healthcare systems expose data models that do not align with the neurology domain's ubiquitous language.

Without anti-corruption layers, the neurology domain model would gradually accumulate concepts from laboratory systems, hospital ADT systems, pharmacy platforms, and external EHR systems, leading to a model that no longer faithfully represents neurology clinical practice.

## ACL Design Principles

### Translation, Not Adaptation

The anti-corruption layer translates external concepts into the neurology domain's ubiquitous language. It does not adapt the neurology model to match external systems. For example, when a laboratory system returns a "specimen result" with a "test code," the ACL translates this into a neurology-meaningful concept like "CSF Biomarker Result" with domain-specific attributes (amyloid-beta level, tau protein concentration, neurofilament light chain value).

### Isolation of External Models

External system models are confined to the ACL boundary. No external system's data model should appear in the neurology domain's aggregates, entities, or value objects. The ACL is the only component that understands both the external model and the internal domain model.

### Bidirectional Translation

Anti-corruption layers handle both inbound and outbound translation. Inbound translation converts external data into domain concepts. Outbound translation converts domain commands into external system requests.

## ACL Implementations in the Neurology Domain

### Laboratory System ACL

The laboratory system ACL translates between generic laboratory data models and neurology-specific biomarker concepts.

Inbound translations:
- Lab "test result" with code "CSF-AB42" becomes a CerebrospinalFluidBiomarkerResult value object with amyloidBeta42Level.
- Lab "genetic test result" with variant data becomes a GeneticVariantReport value object with pathogenicity classification.
- Lab "serum level" for therapeutic drug monitoring becomes an AEDSerumLevel value object in the Epilepsy Management Context.

Outbound translations:
- A BiomarkerOrderRequest from the Neurodegenerative Disease Context becomes a laboratory "test order" with appropriate specimen requirements and test codes.

### Hospital ADT System ACL

The hospital ADT (Admission, Discharge, Transfer) system ACL translates generic hospital concepts into neurology-specific care concepts.

Inbound translations:
- ADT "admission" event with department code becomes a NeurologyAdmission with neurological level of care (neuro-ICU, stroke unit, epilepsy monitoring unit, general neurology).
- ADT "transfer" event becomes an internal context event appropriate to the destination unit.
- ADT "discharge" event becomes a NeurologyDischarge with neurological disposition (home, rehabilitation facility, skilled nursing, hospice).

### Pharmacy System ACL

The pharmacy ACL translates between generic medication data models and neurology-specific therapeutic concepts.

Inbound translations:
- Pharmacy "drug order status" becomes an AEDPrescriptionStatus in Epilepsy Management or a DMTAdministrationStatus in Neurodegenerative Disease.
- Pharmacy "drug interaction alert" becomes a NeurologicDrugInteractionWarning with seizure-threshold and neurological side-effect context.

Outbound translations:
- An AEDPrescription from Epilepsy Management becomes a pharmacy "medication order" with appropriate RxNorm coding and dosing parameters.

### External EHR System ACL

When the neurology domain integrates with external electronic health record systems (for referrals, shared care, or health information exchange), an ACL translates external clinical documents into neurology domain concepts.

Inbound translations:
- External "clinical document" (CDA or FHIR DocumentReference) becomes a ReferralSummary with extracted neurological history, examination findings, and imaging results.
- External "problem list" entries become NeurologicDiagnosis value objects mapped to the neurology domain's diagnostic taxonomy.

### Radiology PACS ACL

While the Neuroimaging Context natively supports DICOM, an ACL exists for non-DICOM imaging sources.

Inbound translations:
- Non-DICOM imaging data (external CD imports, scanned films) is translated into DICOM-compliant objects within the Neuroimaging Context.
- External radiology reports in free-text format are parsed into structured findings compatible with the Neuroimaging Context's report model.

## ACL Implementation Patterns

Anti-corruption layers in the neurology domain use several implementation patterns:

1. **Translator**: Stateless services that convert between external and internal representations.
2. **Adapter**: Interface adapters that expose domain-friendly APIs while consuming external system APIs.
3. **Facade**: Simplified interfaces that hide the complexity of external system interactions.

Each ACL component is owned by the neurology bounded context it protects, not by the external system.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on integration with anti-corruption layers.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on anti-corruption layer patterns.
- HL7 International. FHIR R4 resource mapping specifications.
