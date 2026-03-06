# Regulatory Compliance in Agile Management

## Overview

Regulatory compliance addresses the legal, governance, and audit requirements that shape how agile processes are designed and documented. Agile teams operating in regulated industries (finance, healthcare, defense, government) must satisfy compliance frameworks that mandate traceability, change control, audit trails, and process maturity assessments. In Domain-Driven Design, regulatory requirements are cross-cutting concerns that influence aggregate invariants, domain event design, and repository implementation without being confined to a single bounded context.

## Relevance to Agile Management

A common misconception is that agile and compliance are incompatible -- that agile's emphasis on flexibility conflicts with compliance's demand for control. In practice, well-designed agile domain models inherently support compliance requirements. Domain events provide audit trails. Aggregate invariants enforce business rules. Repositories preserve history. The key is to embed compliance requirements into the domain model as first-class concepts rather than treating them as afterthoughts bolted on through external tooling.

## Key Compliance Frameworks

### SOX -- Sarbanes-Oxley Act

SOX requires public companies to maintain internal controls over financial reporting, including change management controls for systems that affect financial data.

**Agile Impact**:
- **Audit Trails**: Every change to production systems must be traceable to an authorized request. Domain events (StoryCompleted, ReleaseDeployed, FeatureFlagToggled) provide the immutable event stream that auditors require.
- **Segregation of Duties**: The person who writes code should not be the same person who approves deployment. The Release Management Context enforces this through deployment approval workflows that require separate authorization.
- **Change Authorization**: All production changes must be authorized through a formal change management process. Sprint backlog items serve as the authorization record -- only items approved through sprint planning can be deployed.
- **Domain Model Impact**: The Release aggregate requires additional invariants: deployment must have explicit approver (different from developer), change request ID must link to an approved backlog item, and all deployment events must be immutably recorded.

### CMMI -- Capability Maturity Model Integration

CMMI is a process improvement framework that assesses organizational maturity across five levels: Initial, Managed, Defined, Quantitatively Managed, and Optimizing. CMMI process areas overlap significantly with agile practices when mapped correctly.

**Agile Impact**:
- **Process Documentation**: CMMI requires defined processes. The bounded context definitions, aggregate invariants, and domain event catalogs serve as process documentation.
- **Measurement and Analysis**: CMMI Level 4 requires quantitative process management. The Continuous Improvement Context's metrics (cycle time, lead time, throughput, velocity) satisfy this requirement.
- **Process Improvement**: CMMI Level 5 requires continuous process optimization. Retrospectives and Kaizen practices provide the improvement mechanism.
- **Domain Model Impact**: The Continuous Improvement Context must maintain historical metric data with sufficient granularity for statistical process control analysis.

### ISO 27001 -- Information Security Management

ISO 27001 establishes requirements for an information security management system (ISMS), including access control, change management, and incident response.

**Agile Impact**:
- **Access Control**: Sprint backlog items involving security-sensitive changes require additional review gates. The Sprint aggregate may include a SecurityReviewRequired flag.
- **Change Management**: All changes to information systems must follow a documented change management process. The sprint lifecycle (planning, execution, review, deployment) provides this process.
- **Incident Response**: Security incidents discovered during sprint execution must be handled through a defined response process. StoryBlocked events with a security classification trigger escalation workflows.
- **Domain Model Impact**: The Sprint aggregate requires security classification for backlog items. The Release aggregate requires security scan results as a deployment prerequisite.

### Change Management (ITIL / ITSM)

ITIL change management classifies changes as standard (pre-approved, low-risk), normal (requires change advisory board approval), or emergency (expedited approval for urgent fixes).

**Agile Impact**:
- **Standard Changes**: Sprint backlog items that fall within pre-approved change categories can be deployed without additional approval. Feature flags for gradual rollout qualify as standard changes in many organizations.
- **Normal Changes**: Sprint backlog items that introduce significant system changes require change advisory board (CAB) review. The Release Management Context integrates CAB approval as a deployment gate.
- **Emergency Changes**: Hotfixes that bypass the sprint cycle require expedited approval. The domain model must support an emergency change path with appropriate controls and post-deployment review.
- **Domain Model Impact**: The Release aggregate includes a ChangeType classification (Standard, Normal, Emergency) that determines the required approval workflow.

## Compliance in the Domain Model

### Audit Trail Through Domain Events

Domain events are immutable records of everything that happened in the system. An event store containing SprintStarted, StoryCompleted, ReleaseDeployed, and FeatureFlagToggled events provides a complete, tamper-proof audit trail. Each event includes a timestamp, actor (who triggered the event), and correlation ID (linking related events across contexts). This satisfies audit trail requirements across SOX, CMMI, and ISO 27001.

### Traceability Through Aggregate References

Every release item traces back to a completed story (StoryID), which traces back to a backlog item (BacklogItemID), which traces back to a business requirement. This end-to-end traceability -- from business need to production deployment -- satisfies regulatory requirements for change justification.

### Process Control Through Invariants

Aggregate invariants encode compliance rules as domain logic. "A release cannot be deployed without a verified rollback plan" is both a good engineering practice and a compliance requirement. By embedding these rules in the domain model rather than in external checklists, compliance is enforced programmatically rather than manually.

## Compliance Design Guidelines

1. **Embed compliance in the domain model**: Compliance rules are business rules. Model them as aggregate invariants, not as external audit checklists that are separate from the code.

2. **Use domain events for audit trails**: The event stream is the audit trail. Design events with sufficient context (actor, timestamp, correlation ID, before/after state) for audit reconstruction.

3. **Separate compliance concerns by context**: SOX segregation of duties is a Release Management concern. CMMI measurement is a Continuous Improvement concern. ISO 27001 access control spans multiple contexts.

4. **Maintain immutable history**: Repositories should support historical queries. Compliance audits require answering "what was the state at time T?" -- not just "what is the current state?"

5. **Automate compliance verification**: Use domain services to verify compliance rules before allowing state transitions. A deployment approval service checks segregation of duties, change authorization, and security scan results before permitting a ReleaseDeployed event.

## Relationships to Other Topics

- [Domain Events](../domain-events/) -- Domain events provide the immutable audit trail that compliance frameworks require
- [Aggregates](../aggregates/) -- Aggregate invariants encode compliance rules as enforceable business logic
- [Repositories](../repositories/) -- Event-sourced repositories provide historical state reconstruction for audit queries
- [Release Management Context](../release-management-context/) -- Primary context for deployment-related compliance controls
- [Continuous Improvement Context](../continuous-improvement-context/) -- Provides metrics data for CMMI process maturity assessment
- [Agile Standards](../agile-standards/) -- Standards provide the process framework upon which compliance requirements are layered

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Schwaber, Ken and Sutherland, Jeff. "The Scrum Guide." 2020.
- CMMI Institute. "CMMI for Development, Version 2.0." ISACA, 2018.
- ISO/IEC. "ISO/IEC 27001:2022 Information Security Management Systems -- Requirements." International Organization for Standardization, 2022.
- Cohn, Mike. "Agile Estimating and Planning." Prentice Hall, 2005.
- Scaled Agile, Inc. "SAFe and Compliance." scaledagileframework.com.
