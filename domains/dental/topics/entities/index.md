# Entities - Dental Domain

## Overview

Entities are domain objects that have a unique identity which persists over time, even as their attributes change. In the dental domain, entities represent things like patients, teeth, treatment plans, and appointments, each of which maintains continuity of identity across modifications. A tooth entity retains its identity (tooth number 14) even as its condition changes from healthy to carious to restored over the patient's lifetime.

## Entity Design Principles in Dental Systems

Dental entities must capture the temporal nature of clinical data. A tooth's condition changes over time, but it remains the same tooth. A patient's insurance coverage may change annually, but the patient identity persists. Entities in the dental domain must support this kind of identity-based tracking while allowing attribute evolution through clinical events.

## Entities by Bounded Context

### Clinical Examination Context

**Patient**
The Patient entity represents a person receiving dental care. Identity is maintained through a practice-assigned patient identifier. Key attributes include demographic information, medical history relevant to dental treatment (medications, allergies, systemic conditions), and emergency contact information. The patient identity connects records across all bounded contexts.

**Tooth**
The Tooth entity represents a single tooth in a patient's dentition. Identity is based on the combination of patient identifier and tooth number (using universal numbering 1-32 for permanent teeth, A-T for primary teeth). Attributes include current condition, existing restorations, and vitality status. The tooth entity tracks the complete history of interventions and conditions.

**Examination**
The Examination entity represents a single clinical examination event. Identity is based on a generated examination identifier. Attributes include examining provider, date, examination type (comprehensive, periodic, limited), and associated findings. Each examination produces a set of findings that reference specific teeth and structures.

**Radiograph**
The Radiograph entity represents a single radiographic image. Identity is based on an image identifier. Attributes include image type (periapical, bitewing, panoramic, CBCT), acquisition date, projection details, exposure parameters, and the interpreting provider's findings.

### Treatment Planning Context

**Treatment Item**
The Treatment Item entity represents a single proposed procedure within a treatment plan. Identity is based on a treatment item identifier. Attributes include the CDT procedure code, tooth and surface designation, estimated cost, insurance coverage estimate, patient responsibility estimate, clinical priority, and sequencing dependencies on other treatment items.

**Consent Record**
The Consent Record entity represents a patient's documented agreement to a proposed treatment. Identity is based on a consent identifier. Attributes include the referenced treatment items, risks discussed, alternatives presented, patient questions documented, consent date, and signature confirmation.

### Restorative Services Context

**Restoration**
The Restoration entity represents a completed or in-progress restorative procedure. Identity is based on a restoration identifier. Attributes include the procedure type, tooth and surfaces involved, material selected, shade (for aesthetic materials), placement date, and placing provider. Multi-visit restorations track stage progression.

**Implant**
The Implant entity represents a dental implant from surgical placement through prosthetic loading. Identity is based on an implant identifier. Attributes include implant system, diameter, length, placement site, placement date, osseointegration status, and abutment and prosthetic details. The implant entity tracks the long-term lifecycle of the fixture.

### Orthodontic Management Context

**Orthodontic Appliance**
The Orthodontic Appliance entity represents a braces system or aligner set. Identity is based on an appliance identifier. Attributes include appliance type (fixed brackets, clear aligners, lingual), placement date, manufacturer, and current status. For aligner systems, the entity tracks which aligner number is currently active.

**Adjustment Visit**
The Adjustment Visit entity represents a single orthodontic adjustment appointment. Identity is based on a visit identifier. Attributes include visit date, wire changes, elastic configurations, bracket repositioning, progress measurements, and photographic records.

### Periodontal Care Context

**Periodontal Assessment**
The Periodontal Assessment entity represents a complete periodontal evaluation at a point in time. Identity is based on an assessment identifier. Attributes include assessment date, overall disease classification (stage and grade), and the full set of probing measurements. Each assessment provides a snapshot for comparison with previous assessments.

**Periodontal Treatment Session**
The Periodontal Treatment Session entity represents a single periodontal treatment visit. Identity is based on a session identifier. Attributes include date, quadrants treated, instrumentation approach (hand scaling, ultrasonic, or combination), anesthesia used, and provider.

### Practice Management Context

**Appointment**
The Appointment entity represents a scheduled patient visit. Identity is based on an appointment identifier. Attributes include patient, provider, operatory, date, time, duration, procedure type, and status (scheduled, confirmed, arrived, in-progress, completed, cancelled, no-show).

**Insurance Policy**
The Insurance Policy entity represents a patient's dental insurance coverage. Identity is based on a policy identifier combining carrier, group number, and subscriber identifier. Attributes include coverage effective dates, benefit maximums, deductible amounts, and fee schedules.

## Entity Identity Management

Dental entities require careful identity management because the same real-world object may exist in multiple bounded contexts with different identifiers. A patient has a practice-assigned ID, an insurance subscriber ID, and potentially a state dental board case number. The domain model maintains context-specific identifiers while supporting correlation across contexts when needed.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on entities and identity.
