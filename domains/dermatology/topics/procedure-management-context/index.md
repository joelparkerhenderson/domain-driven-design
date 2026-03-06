# Procedure Management Context

The Procedure Management Context is a core bounded context in the dermatology domain that manages the execution of specialized dermatologic procedures. It owns Mohs micrographic surgery stages and layer tracking, cryotherapy session parameters, laser therapy protocols and equipment settings, phototherapy (UVB/PUVA) treatment plans, and electrosurgery records. This context models the procedural expertise and protocol-driven workflows that distinguish specialized dermatologic care.

## Context Purpose

The Procedure Management Context exists to capture and enforce the precise parameters, sequential steps, safety constraints, and clinical protocols that govern dermatologic procedures. Each procedure type has unique requirements: Mohs surgery involves iterative tissue excision with real-time microscopic examination, cryotherapy requires specific freeze-thaw cycle management, laser therapy demands precise wavelength and fluence control, and phototherapy follows cumulative dose protocols. This context ensures that procedural knowledge is modeled with the rigor and specificity it demands.

## Aggregates

### Procedure Session Aggregate

The primary aggregate is the Procedure Session. The root entity carries the session identifier, procedure type, patient reference, performing provider, procedure date, anatomical site, and session status. Within this aggregate, procedure-type-specific child entities capture the unique parameters and steps for each modality.

For Mohs surgery, MohsStage entities track each layer excision with tissue mapping, section identification, margin status per section, and the decision to proceed or conclude. The aggregate enforces the invariant that each stage must complete microscopic examination and margin assessment before a subsequent stage can begin.

For cryotherapy, CryotherapyApplication entities record each treatment application with freeze duration, thaw period, number of cycles, spray technique, and estimated tissue temperature. The aggregate validates that cycle parameters fall within established clinical ranges.

For laser therapy, LaserPass entities capture each treatment pass with Laser Settings value objects (wavelength, pulse duration, fluence, spot size, repetition rate). The aggregate enforces that laser parameters remain within the safe operating envelope for the specific device and treatment indication.

For electrosurgery, ElectrosurgeryApplication entities record the mode (electrodesiccation, electrofulguration, electrosection, electrocoagulation), power setting, and electrode type.

### Phototherapy Plan Aggregate

The Phototherapy Plan aggregate manages multi-session light treatment protocols. The root entity defines the protocol type (narrowband UVB, broadband UVB, or PUVA), the starting dose based on the patient's minimum erythema dose (MED) or skin type, the dose escalation schedule, maximum single-session dose, and maximum cumulative dose. PhototherapySession entities within the aggregate record each individual treatment: actual dose delivered, treatment duration, body areas exposed, any erythema response, and adverse reactions. The aggregate enforces dose escalation rules, prevents exceeding maximum doses, and tracks cumulative exposure across the treatment course.

## Key Entities

### Procedure Session Entity

The Procedure Session entity transitions through states: scheduled (procedure planned with date and provider), prepared (pre-procedure checklist and setup completed), in-progress (procedure actively being performed), completed (all procedural steps finished), and documented (procedure note and outcomes recorded). The entity accumulates data during execution as individual steps are performed.

### Mohs Stage Entity

Each Mohs Stage represents a single excision-examination cycle. The entity carries the stage number, tissue map identifying excised sections, ink color coding for orientation, and margin assessment results for each section. The stage concludes with either a "clear margins" finding (procedure can end) or "positive margins" finding (additional stage required with targeted excision). This entity captures the iterative decision-making that defines Mohs surgery.

### Phototherapy Session Entity

Each Phototherapy Session records a single light treatment within a plan. The entity carries the planned dose, actual delivered dose, treatment duration, body surface areas treated, pre-treatment psoralen administration (for PUVA), erythema assessment at 24-48 hours post-treatment, and any adverse events. The entity's post-treatment assessment informs the dose adjustment for the next session.

## Key Value Objects

The Laser Settings value object captures the full parameter set for a laser device: wavelength in nanometers, pulse duration in milliseconds, fluence in joules per square centimeter, spot size in millimeters, repetition rate in hertz, and cooling parameters. The Cryotherapy Parameters value object captures freeze duration, thaw time, number of cycles, and spray distance. The Phototherapy Dose value object carries the dose value, measurement unit, and light spectrum type.

## Domain Events Published

ProcedureScheduled is published when a procedure date is confirmed. ProcedureCompleted is published when all procedural steps are finished, carrying key outcome parameters. MohsStageCompleted is published after each Mohs stage for surgical workflow coordination. PhototherapySessionCompleted is published after each light treatment for cumulative tracking.

## Domain Events Consumed

ExcisionPlanFinalized from the Lesion Management Context triggers the scheduling of a surgical procedure. BiopsyDecisionMade from Lesion Management triggers biopsy procedure scheduling. LesionDiagnosisConfirmed from Lesion Management may update procedure protocols if the diagnosis changes the required approach.

## Domain Services

The PhototherapyDoseEscalationService calculates the next session dose based on protocol rules, previous session response, and cumulative exposure. The ProcedureEligibilityService evaluates patient eligibility for specific procedures by checking contraindications, current medications, and skin type considerations.

## Procedure Safety and Protocols

This context enforces procedure-specific safety protocols. Mohs surgery requires tissue processing and microscopic examination between stages. Cryotherapy protocols define appropriate freeze times based on lesion type and location. Laser safety includes eye protection requirements, appropriate wavelength selection for skin type, and test spot protocols. Phototherapy protocols include MED testing, dose escalation limits, and cumulative dose tracking to manage long-term UV exposure risks.

## Relationships to Other Contexts

The Lesion Management Context is upstream, providing procedure requests through excision plans and biopsy decisions. The Treatment Outcomes Context is downstream, consuming procedure completion data to initiate outcome tracking. The Clinical Assessment Context provides patient skin type information relevant to procedure parameter selection.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Alam, Murad, and Ratner, Desiree. "Cutaneous Squamous-Cell Carcinoma." New England Journal of Medicine, 2001.
- Rox Anderson, R., and Parrish, John A. "Selective Photothermolysis." Science, 1983.
- Shriner, Duane L., et al. "Mohs Micrographic Surgery." Journal of the American Academy of Dermatology, 1998.
