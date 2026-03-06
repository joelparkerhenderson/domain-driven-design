# Restorative Services Context - Dental Domain

## Overview

The Restorative Services Context manages the execution of restorative dental procedures that repair or replace damaged tooth structure. This context covers direct restorations (fillings), indirect restorations (crowns, bridges, inlays, onlays), dental implants, and endodontic treatments. It receives approved treatment items from the Treatment Planning Context and manages the multi-step clinical workflow for each restorative procedure through to completion, reporting outcomes back to the Clinical Examination Context for chart updates.

## Core Responsibilities

### Direct Restorations

Direct restorations are single-visit procedures where restorative material is placed directly into a prepared tooth cavity. The context manages material selection, cavity preparation documentation, and placement technique. Key material options include amalgam (silver-mercury alloy for posterior teeth), composite resin (tooth-colored material for aesthetic areas), and glass ionomer (fluoride-releasing material for specific clinical situations).

Each direct restoration records the tooth number, surfaces involved (using standard surface notation), material placed, shade selected (for tooth-colored materials), and the restoring provider. The context validates that the material selection is appropriate for the tooth location and cavity classification.

### Indirect Restorations

Indirect restorations require multiple visits because the restoration is fabricated outside the mouth (in a dental laboratory or by CAD/CAM milling). The context manages the multi-stage workflow:

- Preparation visit: Tooth preparation, impression or digital scan, shade selection, temporary restoration placement.
- Laboratory phase: Tracking fabrication status with the dental laboratory.
- Cementation visit: Temporary removal, try-in, adjustment, and permanent cementation.

For bridges, the context tracks multiple abutment teeth and pontic specifications as a single prosthetic unit. The context enforces the invariant that cementation cannot occur before the laboratory restoration is received and inspected.

### Dental Implants

Dental implant management tracks the surgical and prosthetic phases of implant treatment. The surgical phase includes implant fixture placement with specifications (system, diameter, length, and site). The healing phase monitors osseointegration over a prescribed period (typically 3-6 months). The prosthetic phase covers abutment selection, impression, and prosthetic crown or bridge fabrication and placement.

The context enforces that prosthetic loading cannot begin until the osseointegration assessment confirms adequate bone integration of the implant fixture.

### Endodontic Treatment

Endodontic treatment (root canal therapy) removes infected or necrotic dental pulp, disinfects the root canal system, and fills the canals with obturation material. The context manages the treatment stages:

- Access opening through the crown of the tooth.
- Canal identification and working length determination.
- Canal instrumentation (cleaning and shaping).
- Obturation (filling the canals with gutta percha and sealer).
- Referral for definitive restoration (typically a crown).

Multi-canal teeth require tracking of each canal independently. The context enforces that all identified canals must be instrumented and obturated before the case is completed.

## Aggregate Roots

**Restorative Case**: Tracks a single restorative procedure or related group of procedures through their multi-stage workflow. Enforces staging invariants and material compatibility rules.

**Endodontic Case**: Tracks root canal therapy from access through obturation. Enforces canal-level completion tracking and working length validation.

## Key Entities

- Restoration: A completed or in-progress restorative procedure with material and technique details.
- Implant: A dental implant fixture tracking surgical placement through prosthetic completion.
- Crown: An indirect restoration tracking preparation, fabrication, and cementation stages.
- Bridge: A multi-unit prosthetic tracking abutment and pontic specifications.

## Key Value Objects

- Material Specification: Type, manufacturer, shade, and lot number for traceability.
- Surface Designation: Surfaces involved in a restoration procedure.
- Implant Specification: System, diameter, length, and surface treatment type.
- Canal Working Length: Measured distance from reference point to canal terminus.

## Domain Events Produced

- RestorationCompleted: Updates the clinical chart and triggers billing.
- ImplantPlaced: Records surgical placement on the clinical chart.
- EndodonticTreatmentCompleted: Updates tooth vitality status and enables restorative follow-up.
- IndirectRestorationStageCompleted: Tracks progress through multi-visit procedures.

## Domain Events Consumed

- TreatmentPlanApproved: Receives authorized restorative procedures for scheduling.
- InformedConsentObtained: Verifies consent before procedure initiation.
- TreatmentPlanModified: Adjusts pending procedures based on plan changes.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Summitt, James B., et al. Fundamentals of Operative Dentistry: A Contemporary Approach. Quintessence, 2006.
