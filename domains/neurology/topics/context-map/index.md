# Context Map - Neurology Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a domain. In the neurology domain, the context map documents how the six bounded contexts communicate, share data, and maintain their boundaries while supporting cohesive patient care.

The context map captures both the technical integration patterns and the organizational relationships between the clinical teams that own each bounded context.

## Context Relationships

### Clinical Assessment <-> Neuroimaging (Customer-Supplier)

The Clinical Assessment Context acts as the customer, and the Neuroimaging Context acts as the supplier. Clinical Assessment orders imaging studies and consumes interpretation results. The Neuroimaging Context defines the published language for imaging reports, and Clinical Assessment must conform to this format when consuming results.

The ordering interface defines what clinical information must accompany an imaging request (clinical indication, relevant history, specific protocol requests). The results interface returns structured findings, impressions, and recommendations.

### Clinical Assessment <-> Neurodegenerative Disease (Shared Kernel)

These two contexts share a kernel of common concepts: the patient clinical profile, baseline neurological status, and cognitive assessment scores. Both contexts contribute to and consume from this shared model. Changes to the shared kernel require agreement between both teams.

The shared kernel includes cognitive screening results (MMSE, MoCA scores), baseline neurological examination findings, and medication lists relevant to both acute assessment and longitudinal disease management.

### Clinical Assessment <-> Epilepsy Management (Customer-Supplier)

Clinical Assessment identifies initial seizure presentations and refers patients to the Epilepsy Management Context. The supplier (Epilepsy Management) provides seizure classification results and treatment recommendations back to the referring context.

### Neuroimaging <-> Neurodegenerative Disease (Conformist)

The Neurodegenerative Disease Context conforms to the Neuroimaging Context's model for imaging results. Disease progression tracking relies on standardized imaging biomarkers (hippocampal volume measurements, amyloid PET SUVr values, white matter lesion counts) as defined by the Neuroimaging Context.

### Neuroimaging <-> Epilepsy Management (Customer-Supplier)

Epilepsy Management orders specialized imaging studies (MRI epilepsy protocol, ictal SPECT, PET) from the Neuroimaging Context. The Neuroimaging Context supplies structured reports with epilepsy-specific findings (cortical dysplasia, mesial temporal sclerosis, concordance with EEG findings).

### Neuromuscular Disorders <-> Neuroimaging (Customer-Supplier)

The Neuromuscular Disorders Context orders electrodiagnostic studies (EMG/NCS) from the Neuroimaging Context. Results include nerve conduction velocities, amplitudes, and EMG findings that are essential for distinguishing axonal from demyelinating pathology.

### Neurorehabilitation <-> Clinical Assessment (Downstream)

Neurorehabilitation receives patient data from Clinical Assessment including stroke severity scores (NIHSS), initial examination findings, and functional baseline assessments. Neurorehabilitation does not feed data back upstream to Clinical Assessment in real-time but publishes functional outcome reports.

### Neurorehabilitation <-> Neurodegenerative Disease (Partnership)

These contexts maintain a partnership relationship for patients with progressive conditions requiring ongoing rehabilitation. Both contexts exchange information about functional status changes, disease progression milestones, and therapy adjustments. Neither context dominates the relationship.

## External System Integration

### Laboratory Information System (Anti-Corruption Layer)

An anti-corruption layer translates laboratory results (CSF analysis, serum biomarkers, genetic test results) from external laboratory systems into the neurology domain model. This prevents laboratory system data models from corrupting the neurology bounded contexts.

### Hospital ADT System (Anti-Corruption Layer)

Patient admission, discharge, and transfer events from the hospital ADT system are translated through an anti-corruption layer. Neurology-specific concepts (neurological level of care, neuro-ICU vs. general neurology ward) are mapped from generic hospital concepts.

### Pharmacy System (Published Language)

Medication data is exchanged using a published language based on RxNorm coding. The Epilepsy Management and Neurodegenerative Disease contexts consume formulary data and publish medication orders using this standardized vocabulary.

### Radiology PACS (Open Host Service)

The Neuroimaging Context exposes an open host service conforming to DICOM standards. External PACS systems can query and retrieve imaging studies through standard DICOM operations (C-FIND, C-MOVE, C-STORE).

## Upstream and Downstream Flows

The primary data flow follows the clinical workflow:

1. Clinical Assessment generates initial findings and orders (upstream).
2. Neuroimaging processes orders and returns results.
3. Specialist contexts (Neurodegenerative Disease, Epilepsy, Neuromuscular) consume assessment and imaging data to make disease-specific decisions.
4. Neurorehabilitation receives patient disposition and severity data to plan rehabilitation.

Domain events flow downstream through this chain, with each context enriching the patient's clinical narrative.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
- HL7 International. FHIR R4 resource specifications for healthcare interoperability.
- DICOM Standards Committee. DICOM networking and service specifications.
