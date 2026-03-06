# Context Map

## Overview

A Context Map in Domain-Driven Design is a visual and conceptual representation of how Bounded Contexts relate to one another. It documents the integration patterns, data flows, and organizational dynamics between contexts. For the OKR domain, the Context Map reveals how objectives flow from creation through tracking, alignment, review, and reporting, and identifies the upstream/downstream relationships and integration patterns that govern these flows.

## The Five OKR Bounded Contexts

The OKR domain comprises these five Bounded Contexts:

1. **Objective Setting Context** (OSC)
2. **Key Result Tracking Context** (KRTC)
3. **Alignment & Cascading Context** (ACC)
4. **Review & Cadence Context** (RCC)
5. **Analytics & Reporting Context** (ARC)

## Context Relationships

### OSC -> KRTC: Customer-Supplier

**Direction**: Objective Setting Context is upstream; Key Result Tracking Context is downstream.

**Pattern**: Customer-Supplier

**Description**: When an objective is created and key results are defined in the OSC, the KRTC receives KeyResultDefined events and uses them to initialize tracking records. The KRTC (customer) may request specific attributes on key results (such as measurability metadata) that the OSC (supplier) agrees to provide. The two teams negotiate the contract, but the OSC has priority in model decisions.

**Shared Data**: Objective ID, Key Result ID, Key Result description, target value, baseline value, measurement type, OKR type (committed/aspirational).

**Integration Mechanism**: Domain events (KeyResultDefined, KeyResultModified) published by OSC and consumed by KRTC via an event bus.

### OSC -> ACC: Customer-Supplier

**Direction**: Objective Setting Context is upstream; Alignment & Cascading Context is downstream.

**Pattern**: Customer-Supplier

**Description**: When objectives are created or modified, the ACC consumes ObjectiveCreated and ObjectiveUpdated events to validate alignment with parent objectives and maintain the alignment tree. The ACC depends on the OSC for canonical objective definitions but manages its own alignment model.

**Shared Data**: Objective ID, parent objective ID (if any), team/owner information, cycle reference.

**Integration Mechanism**: Domain events published by OSC, consumed by ACC. The ACC may call back to OSC via a query interface to resolve objective details during alignment validation.

### RCC -> OSC: Upstream Influence

**Direction**: Review & Cadence Context is upstream for cycle management; Objective Setting Context is downstream.

**Pattern**: Conformist

**Description**: The OSC conforms to the cycle definitions published by the RCC. When a new cycle is opened (CycleOpened event), the OSC enables objective setting for that cycle. When a cycle transitions to Active, the OSC locks new objective creation for that cycle. The OSC accepts the RCC's cycle model without translation.

**Shared Data**: Cycle ID, cycle state, cycle date range.

**Integration Mechanism**: Domain events (CycleOpened, CycleTransitioned, CycleClosed) published by RCC.

### RCC -> KRTC: Customer-Supplier

**Direction**: Review & Cadence Context is upstream for scheduling; Key Result Tracking Context is downstream.

**Pattern**: Customer-Supplier

**Description**: The RCC publishes cycle state transitions and check-in schedules. The KRTC uses CycleClosed events to trigger score finalization. The RCC schedules check-in cadences that the KRTC uses to prompt progress updates.

**Shared Data**: Cycle ID, cycle state, check-in schedule, scoring deadline.

**Integration Mechanism**: Domain events and shared calendar constructs.

### KRTC -> RCC: Feedback Loop

**Direction**: Key Result Tracking Context provides scoring data back to Review & Cadence Context.

**Pattern**: Customer-Supplier (reverse direction for scoring data)

**Description**: After score finalization, the KRTC publishes ScoreFinalized events that the RCC consumes to confirm that all key results in a cycle have been scored, enabling the cycle to transition to Closed state. The RCC also uses score data to inform retrospective sessions.

**Shared Data**: Key Result ID, final score, scoring timestamp.

**Integration Mechanism**: Domain events (ScoreFinalized).

### All Upstream Contexts -> ARC: Conformist

**Direction**: All four operational contexts are upstream; Analytics & Reporting Context is downstream.

**Pattern**: Conformist

**Description**: The ARC subscribes to events from all other contexts and builds read-optimized projections for dashboards and reports. It conforms to the models published by upstream contexts, accepting their terminology and structures without translation. This simplifies the ARC but creates coupling to upstream models.

**Consumed Events**: ObjectiveCreated, KeyResultDefined, CheckInRecorded, ScoreFinalized, CycleOpened, CycleClosed, AlignmentCreated, AlignmentChanged.

**Integration Mechanism**: Event subscription with materialized read models (CQRS pattern).

### ACC -> OSC: Partnership (Advisory)

**Direction**: Bidirectional advisory relationship.

**Pattern**: Partnership

**Description**: The ACC may detect alignment conflicts or gaps and publish AlignmentConflictDetected events. The OSC may respond by flagging objectives that need adjustment. This is a collaborative relationship where both teams work together to maintain strategic coherence. Neither team can succeed without the other, as objectives that are well-formed but misaligned fail to deliver strategic value.

**Integration Mechanism**: Domain events and potentially synchronous queries.

## Context Map Diagram (Textual)

```
                    ┌─────────────────────┐
                    │   Review & Cadence   │
                    │      Context         │
                    │       (RCC)          │
                    └──────┬───┬───────────┘
            CycleOpened/   │   │   CycleClosed/
            Transitioned   │   │   CheckInScheduled
                          ▼   │
    ┌──────────────────────┐  │  ┌──────────────────────┐
    │  Objective Setting   │  │  │  Key Result Tracking  │
    │      Context         │◄─┘  │      Context          │
    │       (OSC)          │     │       (KRTC)           │
    └───┬──────────┬───────┘     └───────┬───────────────┘
        │          │  KeyResultDefined    │
        │          └────────────────────►│
        │ ObjectiveCreated               │ ScoreFinalized
        │ ObjectiveUpdated               │
        ▼                                │
    ┌──────────────────────┐             │
    │ Alignment & Cascading│             │
    │      Context         │◄────────────┘
    │       (ACC)          │ (indirect via ARC)
    └──────────────────────┘
                │
                │  All events flow downstream
                ▼
    ┌──────────────────────┐
    │ Analytics & Reporting│
    │      Context         │
    │       (ARC)          │
    └──────────────────────┘
```

## Integration Patterns Summary

| Upstream | Downstream | Pattern | Key Events |
|----------|-----------|---------|------------|
| OSC | KRTC | Customer-Supplier | KeyResultDefined, KeyResultModified |
| OSC | ACC | Customer-Supplier | ObjectiveCreated, ObjectiveUpdated |
| RCC | OSC | Conformist | CycleOpened, CycleTransitioned |
| RCC | KRTC | Customer-Supplier | CycleClosed, CheckInScheduled |
| KRTC | RCC | Customer-Supplier | ScoreFinalized |
| OSC, KRTC, ACC, RCC | ARC | Conformist | All domain events |
| ACC | OSC | Partnership | AlignmentConflictDetected |

## Organizational Alignment

The Context Map also reflects team structure. Following Conway's Law, the bounded context boundaries should align with team boundaries where possible:

- **Goal-Setting Team**: Owns OSC and contributes to ACC.
- **Tracking & Measurement Team**: Owns KRTC and maintains integrations with KPI/KRI data sources.
- **Platform Team**: Owns RCC (cycle management) and ARC (reporting).
- **Alignment Specialists**: Own ACC and work closely with the Goal-Setting Team.

## Relationships to Other Topics

- **Bounded Contexts**: The contexts mapped here are defined in [Bounded Contexts](../bounded-contexts/).
- **Strategic Design**: Context mapping is a core Strategic Design activity, described in [Strategic Design](../strategic-design/).
- **Ubiquitous Language**: Each context in the map has its own language dialect, defined in [Ubiquitous Language](../ubiquitous-language/).
- **Anti-Corruption Layer**: Where external systems connect to the map, ACLs are needed, as documented in [Anti-Corruption Layer](../anti-corruption-layer/).
- **Domain Events**: The events that flow between contexts are detailed in [Domain Events](../domain-events/).
- **Event-Driven Architecture**: The event-based integration described in this map is implemented through the patterns in [Event-Driven Architecture](../event-driven-architecture/).
- **Integration Patterns**: External system integrations that extend beyond this internal map are covered in [Integration Patterns](../integration-patterns/).

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity (Context Map patterns).
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013. Chapter 3: Context Maps.
- Doerr, John. "Measure What Matters: How Google, Bono, and the Gates Foundation Rock the World with OKRs." Portfolio/Penguin, 2018.
- Niven, Paul R. and Lamorte, Ben. "Objectives and Key Results: Driving Focus, Alignment, and Engagement with OKRs." Wiley, 2016.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
