# Entities in the Urology Domain

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Entities persist over time and their attributes may change, but their identity remains constant. In the urology domain, entities represent clinical objects that must be tracked across the patient's urological care journey, such as a specific biopsy core, a particular surgical case, or a named kidney stone.

## Identity in Urology

Identity in the urology domain is clinically meaningful. A patient is identified by a medical record number. A surgical case is identified by a case number. A biopsy specimen is identified by an accession number assigned by pathology. A kidney stone is identified by its location and temporal relationship to the clinical episode. These identities are assigned at creation and never change, even as the clinical attributes of the entity evolve over time.

## Clinical Assessment Entities

### Patient Entity

The Patient entity is central to the Clinical Assessment Context. Its identity is the medical record number (MRN). Over time, a patient's urological status changes: new symptoms develop, diagnostic tests yield results, and treatment responses are observed. The Patient entity tracks this evolution while maintaining a consistent identity across all encounters and contexts.

### CystoscopyFinding Entity

A CystoscopyFinding entity represents a specific observation made during cystoscopy. Each finding has a unique identifier and records the anatomical location (trigone, dome, lateral wall, ureteral orifice, prostatic urethra), appearance (papillary, sessile, flat, erythematous), size, and clinical impression. The finding persists and may be referenced in follow-up cystoscopies for comparison.

### UrodynamicStudy Entity

A UrodynamicStudy entity represents a specific urodynamic evaluation session. It contains the study date, indication, catheter configurations, and detailed phase recordings (filling cystometry, pressure-flow, EMG). The entity persists as a reference point for treatment decisions and for comparison with future studies.

## Surgical Management Entities

### SurgicalCase Entity

The SurgicalCase entity tracks a surgical episode from scheduling through post-operative follow-up. Its identity is the case number. Attributes include the procedure type, surgeon, anesthesia type, operative duration, estimated blood loss, complications, and disposition. The entity's state evolves from "scheduled" through "in-progress" to "completed" and "follow-up."

### IntraoperativeEvent Entity

An IntraoperativeEvent entity represents a significant occurrence during surgery, such as an injury to an adjacent structure, conversion from laparoscopic to open approach, or unexpected pathological finding. Each event has a unique identifier, timestamp, description, and severity classification. These events are referenced in quality assurance reviews and outcome analyses.

## Oncologic Urology Entities

### BiopsyCore Entity

A BiopsyCore entity represents a single tissue core obtained during a prostate biopsy. Each core has an identity linked to its accession number and core position (systematic location or MRI-fusion target coordinates). Attributes include core length, tumor length, Gleason pattern, percentage involvement, and perineural invasion status. Individual cores are tracked because aggregate biopsy statistics are computed from the collection of core-level data.

### SurveillanceVisit Entity

A SurveillanceVisit entity represents a single monitoring encounter within an active surveillance or post-treatment surveillance program. Each visit has a unique identifier, date, and protocol reference. The visit contains the tests performed (PSA, imaging, biopsy), results obtained, and the surveillance decision rendered (continue, intensify, reclassify, intervene).

## Stone Disease Entities

### StoneEvent Entity

A StoneEvent entity tracks a specific kidney stone episode from presentation through resolution. Each event has an identity, presentation date, stone characteristics at presentation, interventions performed, and outcome (stone-free, residual fragments, recurrence). The entity maintains its identity even as the stone is fragmented and cleared over multiple procedures.

### MetabolicCollection Entity

A MetabolicCollection entity represents a single 24-hour urine collection within a metabolic workup. Each collection has an identity, collection date, and completeness validation (creatinine excretion). The entity records individual analyte values and their interpretations against reference ranges.

## Incontinence Management Entities

### BladderDiaryEntry Entity

A BladderDiaryEntry entity represents a single day's voiding record within a bladder diary. It tracks individual voids (time, volume), incontinence episodes (time, circumstance, severity), fluid intake, and pad usage. Each entry has a date-based identity and contributes to the aggregate diary analysis.

### TherapySession Entity

A TherapySession entity represents a single pelvic floor therapy appointment. It records the exercises performed, biofeedback measurements, electrical stimulation parameters, and patient-reported progress. Sessions are tracked over time to assess rehabilitation trajectory.

## Male Reproductive Health Entities

### SemenSample Entity

A SemenSample entity represents a single semen specimen submitted for analysis. Each sample has a unique accession number, collection date, abstinence interval, and measured parameters (volume, concentration, total count, motility, morphology). Multiple samples from the same patient are tracked independently because variability between samples is clinically significant.

### TestosteroneLevel Entity

A TestosteroneLevel entity represents a single testosterone measurement. It records the date, time of draw (critical because testosterone follows a diurnal pattern), total testosterone, free testosterone, and calculation method (equilibrium dialysis versus calculated). Serial measurements are tracked as distinct entities to monitor treatment response.

## Entity Lifecycle

Urology entities follow defined lifecycles. A SurgicalCase progresses through scheduling, pre-operative clearance, operative execution, and post-operative follow-up. A CancerDiagnosis evolves through initial detection, biopsy confirmation, staging, treatment, and surveillance. Entity state transitions are governed by domain rules that enforce valid progressions and prevent inconsistent states.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
