# Anti-Corruption Layer in Oncology

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from being contaminated by the models and concepts of external systems. In oncology, anti-corruption layers are critical because the oncology domain must integrate with numerous legacy systems, laboratory information systems, pharmacy systems, imaging archives, and tumor registries, each with its own data model and terminology. Without anti-corruption layers, the oncology domain model would degrade into a patchwork of incompatible concepts borrowed from external systems.

## Purpose in Oncology

Oncology systems rarely exist in isolation. They must consume data from laboratory systems that report test results in HL7 message formats, imaging systems that use DICOM standards, pharmacy systems that track drug inventory using NDC codes, and electronic health record systems with their own patient models. Each of these external systems was designed with its own priorities and modeling conventions that do not align with oncology domain concepts.

The anti-corruption layer translates external system concepts into oncology domain concepts at the boundary. It ensures that the internal oncology model remains pure, clinically accurate, and independent of external system implementation details. This translation preserves the integrity of the ubiquitous language within each bounded context.

## Key Anti-Corruption Layers in Oncology

### Laboratory System ACL

The Diagnosis and Staging Context must receive test results from laboratory information systems (LIS). The LIS may represent results as generic name-value pairs with coded identifiers (LOINC codes). The anti-corruption layer translates these generic results into strongly typed oncology value objects. A generic "test result" with LOINC code 85319-2 becomes a typed HER2 Status value object with clinically meaningful interpretations (positive, negative, equivocal).

This translation also handles the semantic gap between laboratory reporting and clinical interpretation. The LIS reports a Ki-67 proliferation index as a percentage. The anti-corruption layer translates this into a clinically categorized value (low, intermediate, high) based on tumor-type-specific thresholds defined in the oncology domain model.

### Pharmacy System ACL

The Chemotherapy Management Context must interface with pharmacy systems for drug dispensing and inventory management. Pharmacy systems typically model drugs using NDC (National Drug Code) identifiers and track inventory in dispensing units. The anti-corruption layer translates between pharmacy drug concepts and oncology regimen concepts.

A pharmacy system sees individual drug products. The oncology domain sees regimens composed of multiple agents with specific dose calculations, administration sequences, and pre-medications. The ACL maps between these perspectives, translating an oncology chemotherapy order into pharmacy-understandable dispensing instructions while keeping the oncology model free of pharmacy-specific concepts like lot numbers and storage requirements.

### Imaging System ACL

The Radiation Therapy Context and the Diagnosis and Staging Context both consume data from imaging systems using DICOM standards. DICOM represents images as series within studies, with metadata about modality, acquisition parameters, and patient positioning. The anti-corruption layer extracts the clinically relevant information and translates it into oncology domain concepts.

For radiation therapy, the ACL translates DICOM-RT structure sets into oncology target volume definitions. For diagnosis and staging, the ACL translates imaging study results into staging-relevant findings (tumor measurements, lymph node assessments, metastatic lesion identification).

### Tumor Registry ACL

Cancer registries require standardized reporting using coding systems such as ICD-O (International Classification of Diseases for Oncology) for topography and morphology. The anti-corruption layer translates the oncology domain model's rich diagnostic information into the flattened, coded format required by registries. This translation is lossy by design; the registry representation is a simplified view of the domain model, and the ACL manages this simplification explicitly.

### Electronic Health Record ACL

The oncology system typically operates within or alongside an electronic health record (EHR) system. The EHR has its own patient model, encounter model, and order model. The anti-corruption layer ensures that the oncology domain's specialized concepts (treatment plans, chemotherapy cycles, radiation fractions) are not forced to conform to the EHR's generic clinical models.

## Design Principles

Anti-corruption layers in oncology should follow several design principles. Translation should be explicit and documented, with clear mapping between external concepts and domain concepts. The ACL should validate incoming data against domain rules, rejecting or flagging data that does not conform to domain expectations. The ACL should handle versioning of external system interfaces, isolating the domain model from changes in external system APIs or data formats.

The ACL should be thin and focused on translation, not business logic. Clinical rules and domain logic belong in the domain model, not in the translation layer. The ACL's responsibility is to produce well-formed domain objects from external data, at which point the domain model takes over.

## Bidirectional Translation

Many oncology ACLs must support bidirectional translation. The Chemotherapy Management Context receives drug information from the pharmacy system and sends chemotherapy orders back. The Diagnosis and Staging Context receives laboratory results and may send order requests. Bidirectional ACLs must maintain consistent translation in both directions, ensuring that concepts mapped from external to internal representations can be accurately mapped back when needed.

## Testing Anti-Corruption Layers

ACL testing in oncology must verify that translation produces clinically correct domain objects. Test cases should include edge cases specific to oncology, such as equivocal biomarker results, amended pathology reports that supersede prior versions, and staging classifications that change between AJCC editions. ACL tests serve as executable documentation of the translation rules between external and domain concepts.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3: Context Maps.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7: Context Mapping Patterns.
