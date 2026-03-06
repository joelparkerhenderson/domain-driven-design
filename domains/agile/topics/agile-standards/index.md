# Agile Standards in Agile Management

## Overview

Agile standards are the established frameworks, methodologies, and guiding principles that define how agile management is practiced. These standards inform the domain model's design by providing the authoritative definitions of concepts such as sprints, backlogs, kanban boards, and ceremonies. In Domain-Driven Design terms, agile standards function as the "published language" -- the shared, codified vocabulary that ensures consistency across bounded contexts and alignment with industry-recognized practices.

## Relevance to Agile Management

Domain models must be grounded in authoritative definitions. When the Sprint Execution Context defines a sprint, that definition should align with the Scrum Guide's specification. When the Continuous Improvement Context tracks WIP limits and flow metrics, those concepts should follow Kanban Method principles. Agile standards provide the canonical source for the ubiquitous language, ensuring that the domain model reflects proven practices rather than ad hoc interpretations.

## Key Standards

### The Agile Manifesto (2001)

The foundational document of the agile movement, authored by seventeen software practitioners. It defines four value pairs and twelve supporting principles:

**Values**:
- Individuals and interactions over processes and tools
- Working software over comprehensive documentation
- Customer collaboration over contract negotiation
- Responding to change over following a plan

**Domain Model Impact**: The Agile Manifesto does not prescribe specific practices, but it establishes the philosophical foundation. Domain models should prioritize collaboration (ubiquitous language), working increments (Definition of Done), and adaptability (event-driven architecture) over rigid process enforcement.

### The Scrum Guide (Schwaber and Sutherland, 2020)

The definitive reference for the Scrum framework. It defines roles (Product Owner, Scrum Master, Developers), events (Sprint, Sprint Planning, Daily Scrum, Sprint Review, Sprint Retrospective), and artifacts (Product Backlog, Sprint Backlog, Increment).

**Domain Model Impact**: The Scrum Guide directly shapes the Sprint Execution Context (sprint lifecycle, ceremonies), Backlog Management Context (Product Backlog, product owner authority), and Continuous Improvement Context (retrospectives). Key invariants -- such as sprint time-box immutability and the requirement for a sprint goal -- derive from the Scrum Guide.

### The Kanban Method (Anderson, 2010)

A method for managing knowledge work that emphasizes visualizing workflow, limiting work in progress (WIP), managing flow, making policies explicit, and pursuing continuous improvement through evolutionary change.

**Domain Model Impact**: The Kanban Method shapes the Continuous Improvement Context (cycle time, lead time, throughput, CFD, WIP limits) and influences the Sprint Execution Context (Kanban board visualization, WIP enforcement). Flow metrics from Kanban complement Scrum's velocity-based planning.

### SAFe -- Scaled Agile Framework

A framework for scaling agile practices across large organizations with multiple teams. SAFe organizes work at four levels: Team, Program, Large Solution, and Portfolio. It introduces concepts such as Agile Release Train (ART), Program Increment (PI), and portfolio-level Kanban.

**Domain Model Impact**: SAFe extends the domain model for organizations with multiple teams. The Release Management Context incorporates the release train concept. The Team & Capacity Context extends to cross-team capacity planning. SAFe introduces additional coordination events (PI Planning, System Demo) that may become domain events in larger-scale models.

### LeSS -- Large-Scale Scrum

A framework for scaling Scrum with minimal additional process. LeSS applies Scrum rules to multiple teams working on a single product, emphasizing simplicity (one Product Backlog, one Product Owner, one Definition of Done across all teams) and cross-team coordination through joint Sprint Planning and joint Retrospectives.

**Domain Model Impact**: LeSS constrains the Backlog Management Context to a single ProductBacklog aggregate shared across teams, requiring the domain model to support multi-team prioritization and refinement within a single aggregate boundary.

### Extreme Programming -- XP (Beck, 1999)

A software development methodology emphasizing technical practices: pair programming, test-driven development (TDD), continuous integration, refactoring, collective code ownership, and simple design. XP provides the engineering discipline that sustains agile delivery.

**Domain Model Impact**: XP practices inform the Definition of Done value object (e.g., code reviewed, tests passing, refactored) and the Release Management Context (continuous integration, automated testing as quality gates). XP's planning game concept aligns with the Backlog Management Context's estimation and prioritization responsibilities.

### Agile Manifesto Principles (2001)

The twelve principles behind the Agile Manifesto expand on the four values, covering topics such as sustainable pace, technical excellence, self-organizing teams, and regular reflection. Key principles with direct domain model impact:

- "Deliver working software frequently" -- shapes the Release Management Context
- "Welcome changing requirements" -- shapes the Backlog Management Context's continuous refinement model
- "At regular intervals, the team reflects" -- shapes the Continuous Improvement Context
- "Build projects around motivated individuals" -- shapes the Team & Capacity Context

## How Standards Inform Domain Design

| Standard | Primary Context Impacted | Key Concepts Adopted |
|----------|------------------------|---------------------|
| Scrum Guide | Sprint Execution, Backlog Management | Sprint, Sprint Goal, Product Backlog, Definition of Done |
| Kanban Method | Continuous Improvement | Cycle Time, Lead Time, WIP Limits, Throughput |
| SAFe | Release Management, Team & Capacity | Release Train, PI Planning, Cross-team Capacity |
| LeSS | Backlog Management | Single Product Backlog, Multi-team Refinement |
| XP | Release Management | TDD, Continuous Integration, Pair Programming |
| Agile Manifesto | All Contexts | Values and Principles as Design Constraints |

## Relationships to Other Topics

- [Ubiquitous Language](../ubiquitous-language/) -- Agile standards provide the authoritative source for domain terminology
- [Bounded Contexts](../bounded-contexts/) -- Standards define the concepts that each bounded context models
- [Value Objects](../value-objects/) -- Standards define the rules for value objects like DefinitionOfDone, WIPLimit, and SprintGoal
- [Aggregates](../aggregates/) -- Aggregate invariants derive from standard-defined rules (e.g., time-box immutability from Scrum Guide)
- [Regulatory Compliance](../regulatory-compliance/) -- Standards provide the process framework that compliance requirements are layered upon

## References

- Beck, Kent et al. "Manifesto for Agile Software Development." agilemanifesto.org, 2001.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." scrumguides.org, 2020.
- Anderson, David. "Kanban: Successful Evolutionary Change for Your Technology Business." Blue Hole Press, 2010.
- Leffingwell, Dean. "SAFe 5.0 Distilled: Achieving Business Agility with the Scaled Agile Framework." Addison-Wesley, 2019.
- Larman, Craig and Vodde, Bas. "Large-Scale Scrum: More with LeSS." Addison-Wesley, 2016.
- Beck, Kent. "Extreme Programming Explained: Embrace Change." Addison-Wesley, 1999.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
