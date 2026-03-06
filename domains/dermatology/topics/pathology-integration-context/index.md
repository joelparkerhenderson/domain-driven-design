# Pathology Integration Context

The Pathology Integration Context is a generic bounded context in the dermatology domain that coordinates the flow of information between clinical dermatology practice and pathology laboratory services. It owns specimen tracking from collection through accessioning, histopathology report integration, chain of custody documentation, and TNM staging for cutaneous malignancies. As a generic subdomain, this context leverages established interoperability standards (HL7, FHIR) and focuses on translation between external laboratory systems and the dermatology domain's internal model.

## Context Purpose

The Pathology Integration Context exists because the definitive diagnosis for many skin conditions depends on histopathological examination performed by pathology laboratories, which are typically external organizations with their own information systems. This context manages the handoff of specimens from clinical practice to laboratory, translates between different data formats and terminologies, tracks specimens through the laboratory workflow, and integrates diagnostic results back into the clinical domain. The context ensures that pathology findings are accurately represented in the dermatology domain's ubiquitous language.

## Aggregates

### Specimen Case Aggregate

The primary aggregate is the Specimen Case. The root entity carries the specimen accession number (assigned upon laboratory registration), the originating biopsy reference (linking back to the Lesion Management Context), the specimen type (shave, punch, incisional, excisional), collection details (date, collecting provider, anatomical site), and clinical history provided to the pathologist. Within this aggregate, ChainOfCustodyEntry entities document each handling transition from collection to laboratory receipt. The HistopathologyReport entity, when completed, carries the pathologist's gross and microscopic descriptions, diagnosis, margin assessment, and any special stain or immunohistochemistry results.

The aggregate enforces several invariants. A specimen must have a complete chain of custody before laboratory processing can begin. The clinical history must include the anatomical site and clinical differential diagnosis to meet pathology accessioning requirements. A final report cannot be issued until all requested special studies are completed. Margin status must be documented for excisional specimens.

## Key Entities

### Specimen Case Entity

The Specimen Case entity transitions through states: collected (specimen obtained from patient), in-transit (specimen being transported to laboratory), accessioned (formally registered in laboratory system with accession number assigned), processing (tissue being fixed, embedded, sectioned, and stained), under-examination (pathologist reviewing slides), reported (preliminary or final report issued), and archived (case completed and filed). Each state transition is documented with a timestamp and responsible party.

### Chain of Custody Entry Entity

Each Chain of Custody Entry documents a single handling event in the specimen's journey. The entity carries the handler identity, handler role, timestamp, action performed (collected, packaged, transported, received, logged, processed), storage condition (ambient, refrigerated, frozen), and condition assessment (intact, compromised). The chain must be unbroken from collection to laboratory receipt.

### Histopathology Report Entity

The Histopathology Report entity captures the pathologist's findings and diagnosis. It carries the gross description (specimen dimensions, appearance, orientation), microscopic description (tissue architecture, cellular characteristics, growth pattern), diagnosis with ICD-10 code, margin status (clear, close, involved, with distances in millimeters), and any ancillary study results. For malignancies, additional fields capture Breslow thickness, Clark level, ulceration status, mitotic rate, lymphovascular invasion, and perineural invasion. The report may include a pathologist's comment with clinical recommendations.

## Key Value Objects

The TNM Classification value object captures the staging for cutaneous malignancies with T category (tumor size and characteristics), N category (regional lymph node status), M category (distant metastasis status), and the derived AJCC stage. For melanoma, the T category incorporates Breslow thickness and ulceration status. The Specimen Type value object classifies the biopsy technique and expected tissue yield. The Accession Number value object carries the laboratory-specific specimen identifier and the laboratory source.

## Domain Events Published

SpecimenAccessioned is published when the laboratory formally registers a specimen, confirming receipt and providing the accession number. PathologyReportCompleted is published when the pathologist finalizes the diagnostic report, carrying the diagnosis, staging information, and clinical recommendations. SpecimenInTransit is published when a specimen leaves the clinical site for transport to the laboratory.

## Domain Events Consumed

BiopsyPerformed from the Lesion Management Context triggers the creation of a new Specimen Case and initiates specimen tracking. The event provides clinical context (patient information, anatomical site, clinical impression, prior diagnoses) that accompanies the specimen to the laboratory.

## Domain Services

The StagingClassificationService determines TNM staging based on histopathology report findings, applying AJCC staging criteria for specific malignancy types. The service correlates tumor measurements, invasion depth, ulceration status, and clinical findings to produce the appropriate TNM Classification value object.

## Anti-Corruption Layer

The most critical design element of this context is the anti-corruption layer that interfaces with external laboratory information systems. Laboratories communicate using HL7 v2 messages (ORM orders, ORU results), FHIR resources, or proprietary formats. The ACL translates these external representations into the domain's internal model and vice versa.

Inbound translation converts laboratory result messages into HistopathologyReport entities, mapping laboratory-specific diagnostic codes to the domain's ICD-10 and SNOMED CT terminology. Outbound translation converts Specimen Case data into the order format required by each laboratory, mapping the domain's clinical context into the laboratory's required fields.

The ACL handles discrepancies between laboratory and domain terminology. A laboratory may report "squamous cell carcinoma, well-differentiated" while the domain model requires structured fields for differentiation grade, depth of invasion, and perineural involvement. The ACL parses and structures the laboratory's text-based findings into the domain's structured representation.

## Multiple Laboratory Support

The dermatology domain may work with multiple pathology laboratories. The anti-corruption layer supports multiple laboratory adapters, each handling the specific communication protocol and data format of its laboratory. A laboratory routing component determines which laboratory adapter to use based on specimen type, insurance requirements, and laboratory capabilities (such as dermatopathology subspecialty expertise).

## Specimen Tracking Workflow

The typical specimen workflow begins when a biopsy is performed in the clinical setting (documented in the Lesion Management Context). The BiopsyPerformed event triggers creation of a Specimen Case. The specimen is packaged with a requisition containing clinical history, placed in the chain of custody with the initial collection entry, and transported to the laboratory. Upon laboratory receipt, the accessioning event confirms the specimen is in the laboratory system. Processing, examination, and reporting follow the laboratory's internal workflow, with the final report being translated through the ACL and published as a PathologyReportCompleted event.

## Relationships to Other Contexts

The Lesion Management Context is upstream, initiating specimen tracking through BiopsyPerformed events. The Lesion Management Context is also downstream, consuming PathologyReportCompleted events to update lesion diagnoses. The Treatment Outcomes Context consumes pathology data for staging information that informs outcome tracking. External laboratory information systems interface through the anti-corruption layer.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Rapini, Ronald P. *Practical Dermatopathology*. Elsevier, 2nd Edition, 2012.
- College of American Pathologists. *Cancer Protocol Templates for Cutaneous Malignancies*.
- Health Level Seven International. *HL7 Version 2 Messaging Standard*.
