# Event-Driven Architecture in Cardiology

## Overview

Event-driven architecture (EDA) enables communication between the six cardiology bounded contexts through domain events rather than direct, synchronous calls. In the cardiology domain, EDA reflects the natural flow of clinical information: a patient is assessed, imaging is ordered and completed, findings trigger procedural referrals, procedures generate follow-up requirements, and rehabilitation programs are designed based on accumulated clinical data. Each step produces events that downstream contexts consume asynchronously.

## Rationale for Event-Driven Architecture in Cardiology

Cardiology workflows are inherently event-driven. Clinical care proceeds through a series of significant occurrences -- a troponin result returns elevated, an echocardiogram reveals reduced ejection fraction, a stent is implanted, a device fires -- each triggering downstream actions. EDA captures this natural clinical flow while maintaining loose coupling between bounded contexts.

Key benefits in the cardiology domain:

- **Temporal decoupling**: An imaging study can be completed and its event published independently of whether the Heart Failure Management Context is ready to process it. Events are stored until consumers process them.
- **Scalability**: Multiple contexts can independently consume the same event. When a ProcedureCompleted event is published, Clinical Assessment, Heart Failure Management, and Cardiac Rehabilitation all process it independently.
- **Audit trail**: Events provide a natural clinical audit trail. The sequence of events for a patient tells the story of their cardiac care journey.
- **Resilience**: If the Cardiac Rehabilitation Context is temporarily unavailable, events queue until it recovers, without blocking upstream contexts.

## Event Flow Patterns

### Clinical Pathway: Chest Pain Evaluation

1. **PatientAssessmentCompleted** (Clinical Assessment): Patient presents with chest pain; HEART score calculated as high risk.
2. **CriticalBiomarkerDetected** (Clinical Assessment): Serial troponin shows significant rise-and-fall pattern.
3. **ImagingStudyCompleted** (Diagnostic Imaging): Echocardiogram shows new wall motion abnormality.
4. **ProcedureCompleted** (Interventional Procedures): Urgent catheterization reveals severe LAD stenosis; PCI with stent performed.
5. **StentImplanted** (Interventional Procedures): Drug-eluting stent placed in proximal LAD.
6. **HeartFailureClassificationChanged** (Heart Failure Management): Post-MI ejection fraction of 35% triggers HFrEF classification.
7. **RehabilitationPhaseCompleted** (Cardiac Rehabilitation): Patient completes Phase I inpatient rehabilitation.

### Clinical Pathway: Heart Failure Optimization

1. **EjectionFractionChanged** (Diagnostic Imaging): Follow-up echo shows LVEF decline from 45% to 30%.
2. **HeartFailureClassificationChanged** (Heart Failure Management): Reclassified from HFmrEF to HFrEF.
3. **GDMTOptimizationAchieved** (Heart Failure Management): Patient titrated to target doses on all four GDMT pillars.
4. **DeviceImplanted** (Electrophysiology): CRT-D implanted for persistent reduced EF despite optimized GDMT.
5. **EjectionFractionChanged** (Diagnostic Imaging): Post-CRT echo shows LVEF improvement to 40%.
6. **ExerciseCapacityImproved** (Cardiac Rehabilitation): MET capacity improves from 4 to 7 during rehabilitation.

### Clinical Pathway: Atrial Fibrillation Management

1. **ArrhythmiaDetected** (Electrophysiology): New-onset atrial fibrillation identified on monitoring.
2. **RiskStratificationUpdated** (Clinical Assessment): CHA2DS2-VASc score calculated; anticoagulation initiated.
3. **AblationCompleted** (Electrophysiology): Pulmonary vein isolation performed with acute success.
4. **RehabilitationPhaseCompleted** (Cardiac Rehabilitation): Post-ablation activity guidance provided.

## Event Design Principles

### Event Granularity

Events should be granular enough to carry specific clinical meaning. "StentImplanted" is preferred over generic "ProcedureUpdated" because downstream consumers need to know specifically that a stent was placed (affecting antiplatelet therapy, activity restrictions, and follow-up scheduling). However, events should not be so granular that they create excessive coupling.

### Event Payload Design

Each event carries sufficient data for consumers to act without querying back to the publisher. A StentImplanted event includes patient identifier, procedure identifier, lesion location, stent type and dimensions, and antiplatelet therapy requirements. This avoids temporal coupling where a consumer must synchronously query the publisher.

### Event Ordering and Idempotency

Consumers must handle out-of-order event delivery and duplicate events. If an EjectionFractionChanged event arrives before the ImagingStudyCompleted event that produced it, the consumer must handle this gracefully. Idempotent event handlers ensure that processing the same event twice does not corrupt domain state.

### Event Versioning

As the cardiology domain model evolves (new guideline recommendations, new measurement types), event schemas must evolve without breaking existing consumers. Backward-compatible schema evolution and event version negotiation prevent integration failures when bounded contexts are updated independently.

## Event Store Considerations

An event store captures the complete history of domain events, enabling:

- **Clinical audit reconstruction**: Replay events to understand the complete clinical decision chain.
- **Temporal queries**: Answer questions like "What was this patient's GDMT regimen when their EF was last measured?"
- **Event sourcing**: For aggregates where full history is clinically important (medication titration history, device programming history).

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapters 8 and 13 on domain events and integration.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 10 on event-driven architecture.
- Brandolini, Alberto. *Introducing EventStorming*. Leanpub, 2021.
- Fowler, Martin. "Event Sourcing." martinfowler.com, 2005.
