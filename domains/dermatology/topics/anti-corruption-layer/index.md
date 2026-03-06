# Anti-Corruption Layer in the Dermatology Domain

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, data formats, and model assumptions. In the dermatology domain, anti-corruption layers are essential at integration points where the domain model interfaces with laboratory information systems, imaging platforms, electronic health records, insurance billing systems, and pharmaceutical databases. As Evans (2003) describes, the ACL prevents external models from corrupting the carefully designed domain model.

## Purpose in Dermatology

Dermatology systems interact with numerous external systems, each with its own data model, terminology, and constraints. Without anti-corruption layers, the domain model would be forced to accommodate the idiosyncrasies of every external system, leading to a muddled and inconsistent model. The ACL translates between external representations and the domain's ubiquitous language, preserving model integrity while enabling necessary integration.

## Pathology Laboratory Integration ACL

The most significant anti-corruption layer in the dermatology domain sits between the Pathology Integration Context and external laboratory information systems (LIS). Pathology laboratories use HL7 v2 messages, proprietary result formats, and laboratory-specific terminology that differs from the dermatology domain's internal model.

The ACL translates inbound HL7 ORM (order) and ORU (result) messages into domain concepts. An HL7 OBX segment containing a pathology result is translated into a HistopathologyReport value object with structured findings, a diagnosis classification, and staging information expressed in the domain's ubiquitous language. Outbound translation converts a BiopsySpecimen aggregate into the HL7 order format expected by the laboratory, mapping domain-specific lesion identifiers and clinical context into the laboratory's required fields.

This ACL also handles terminology mapping. The laboratory may report findings using SNOMED CT codes while the domain model uses ICD-10 dermatology codes internally. The ACL maintains the mapping between these classification systems, ensuring that the domain model always works with its preferred terminology.

## Electronic Health Record ACL

When the dermatology domain integrates with a broader electronic health record (EHR) system, an ACL translates between the EHR's patient model and the dermatology domain's patient skin profile. The EHR may represent a patient as a generic medical record with demographics, problem lists, and medication histories. The ACL extracts relevant dermatology information (skin-related allergies, photosensitivity medications, previous skin cancer history) and translates it into the domain's patient representation.

The EHR ACL also handles the reverse translation, converting dermatology-specific documentation (dermoscopy findings, procedure notes, cosmetic treatment records) into the structured formats required by the EHR for compliance and continuity of care.

## Insurance and Billing ACL

Dermatology billing requires translation between clinical domain concepts and payer-specific requirements. The ACL translates procedure aggregates into CPT codes, diagnosis entities into ICD-10 codes with appropriate specificity, and treatment plans into prior authorization requests. Different payers may require different code combinations, modifiers, and documentation for the same clinical scenario. The ACL encapsulates these payer-specific rules, preventing billing complexity from contaminating the clinical domain model.

## Imaging System ACL

Dermatology relies on clinical photography and dermoscopic imaging systems that store images in DICOM format or proprietary formats. The ACL translates between the imaging system's file structure, metadata formats, and storage conventions and the domain's clinical photo documentation model. Image identifiers, anatomical location tags, and acquisition parameters are translated into domain value objects that support the Treatment Outcomes Context's photo comparison functionality.

## Pharmaceutical Database ACL

When the Cosmetic Services Context integrates with pharmaceutical databases for injectable product information, an ACL translates between the manufacturer's product data (NDC codes, package sizes, storage requirements) and the domain's product lot tracking model. This ensures that the domain model represents injectable products in terms meaningful to clinical practice rather than in pharmaceutical distribution terminology.

## Design Principles

Following Vernon's (2013) guidance, the anti-corruption layer in the dermatology domain adheres to several design principles. The ACL is owned by the downstream context that it protects. Translation logic is isolated in dedicated adapter components that can be modified independently of the domain model. The ACL handles both structural translation (data format conversion) and semantic translation (meaning and terminology mapping). Error handling within the ACL converts external system errors into domain-meaningful exceptions.

## Implementation Considerations

Khononov (2021) notes that anti-corruption layers add complexity and should be used judiciously. In the dermatology domain, ACLs are warranted at boundaries with external systems that the domain team does not control and whose models are fundamentally different from the domain model. Within the dermatology domain itself, simpler integration patterns (shared kernel, published language) are preferred between bounded contexts that share the same team and ubiquitous language.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on anti-corruption layer pattern.
- Health Level Seven International. *HL7 Version 2 Messaging Standard*.
- HL7 FHIR. *Fast Healthcare Interoperability Resources Specification*.
