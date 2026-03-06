# Anti-Corruption Layer in the Gynecology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that prevents external system concepts, data structures, and terminology from leaking into a bounded context's domain model. In the gynecology domain, ACLs are essential wherever bounded contexts integrate with external clinical systems such as laboratory information systems, radiology platforms, electronic health records, and pharmacy systems.

## Purpose

Gynecology systems must integrate with many external platforms that use their own data models, code systems, and terminology. Without an anti-corruption layer, external concepts infiltrate the domain model, making it increasingly difficult to maintain a clean, clinically meaningful internal representation. The ACL acts as a translator, converting external data into the bounded context's own ubiquitous language.

## ACL Patterns in the Gynecology Domain

### Laboratory Integration ACL

The Screening Programs Context must receive test results from laboratory information systems (LIS). External lab systems use coding schemes such as LOINC for test identifiers and SNOMED CT for result classifications. The ACL translates these external codes into the Screening Programs Context's own value objects:

- External LOINC codes are translated into ScreeningTestType value objects.
- External result classifications are translated into ScreeningResult value objects with domain-specific categories (Normal, Abnormal, Indeterminate).
- External specimen identifiers are mapped to internal ScreeningEpisode references.

### Radiology Integration ACL

The Prenatal Care Context integrates with radiology and ultrasound systems that produce DICOM-formatted imaging data and HL7-formatted reports. The ACL translates these into domain value objects:

- DICOM metadata is translated into UltrasoundAssessment value objects containing fetal measurements, amniotic fluid index, and placental position.
- HL7 report segments are translated into structured ImagingFindings value objects.
- External patient identifiers are resolved to internal prenatal record references.

### Electronic Health Record ACL

All bounded contexts integrate with an electronic health record (EHR) system. The EHR uses its own patient model, encounter model, and clinical data structures. Each bounded context's ACL translates EHR data into its own model:

- The Clinical Assessment Context translates EHR encounter data into ClinicalEncounter entities.
- The Surgical Services Context translates EHR order data into SurgicalCase entities.
- The Reproductive Health Context translates EHR medication data into ContraceptionPlan value objects.

### Pharmacy System ACL

The Reproductive Health Context integrates with pharmacy systems for contraceptive prescriptions and hormonal therapies. The ACL translates pharmacy-specific drug codes (NDC, RxNorm) into domain-specific Medication value objects with attributes relevant to reproductive health, such as hormonal classification and contraceptive efficacy category.

## ACL Implementation Principles

1. The ACL is owned by the downstream bounded context, not by the external system.
2. Translation logic lives in dedicated adapter components, separate from core domain logic.
3. The ACL exposes only domain-meaningful interfaces to the rest of the bounded context.
4. External system changes require only ACL modifications, not changes to the core domain model.
5. The ACL includes validation to reject external data that cannot be meaningfully translated.

## ACL Between Internal Contexts

Anti-corruption layers are also used between internal bounded contexts when their models diverge significantly:

- The Surgical Services Context uses an ACL to translate Clinical Assessment diagnostic impressions into surgical indication classifications.
- The Patient Education Context uses an ACL to translate clinical events from all upstream contexts into educational trigger categories.

## Benefits in the Gynecology Domain

- Clinical domain models remain expressed in gynecological terms rather than being polluted by HL7, DICOM, LOINC, or EHR vendor terminology.
- External system changes or vendor migrations affect only the ACL layer.
- Domain experts can read and validate the domain model without needing to understand external system data formats.
- Testing is simplified because the ACL provides a clear seam for mocking external dependencies.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 9 on anti-corruption layer patterns.
- Health Level Seven International. HL7 FHIR Standard. https://www.hl7.org/fhir/
