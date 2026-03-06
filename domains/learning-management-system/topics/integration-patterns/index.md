# Integration Patterns in Learning Management System

## Overview

Integration patterns in an LMS connect bounded contexts internally and interface with external educational systems such as Student Information Systems (SIS), Learning Record Stores (LRS), content repositories, and institutional platforms.

## Internal Integration (Between Bounded Contexts)

### Published Language

Shared event schemas that contexts agree upon:

- **Enrollment events**: StudentEnrolled, StudentDropped with standardized student and course identifiers
- **Assessment events**: AssessmentGraded with Grade value object
- **Progress events**: CourseCompleted with completion data and final grade

### Anti-Corruption Layers

- **SIS Integration ACL**: Translates SIS student records into LMS Student entities
- **External Content ACL**: Adapts third-party content formats into LMS ContentItem model
- **LTI ACL**: Translates LTI launch data and grade passback into domain models

### Context Mapping Patterns

| Upstream | Downstream | Pattern |
|----------|-----------|---------|
| Course Catalog | Enrollment | Customer-Supplier |
| Assessment | Learner Progress | Customer-Supplier |
| Content Delivery | Learner Progress | Published Language |
| SIS (External) | Enrollment | Conformist with ACL |
| LTI Tools (External) | Content Delivery | ACL |

## External Integration

### Student Information System (SIS)

- **Direction**: Bidirectional
- **Data**: Student demographics, program enrollment, course registrations, final grades
- **Pattern**: SIS as upstream authority for student identity; LMS pushes grades back
- **Protocol**: SIS APIs, IMS OneRoster, SIF (Schools Interoperability Framework)

### Learning Record Store (LRS)

- **Direction**: LMS → LRS
- **Data**: xAPI statements capturing learning activities, completions, and results
- **Pattern**: LMS as upstream producer of learning data
- **Protocol**: xAPI (Experience API) REST endpoints

### Content Repositories

- **Direction**: External → LMS
- **Data**: SCORM packages, xAPI content, OER (Open Educational Resources)
- **Pattern**: LMS consumes content via standard packaging formats
- **Protocol**: IMS Common Cartridge, LTI Content Item

### Authentication/Identity

- **Direction**: IdP → LMS
- **Data**: User identity, roles, group memberships
- **Pattern**: LMS as SAML/OIDC service provider
- **Protocol**: SAML 2.0, OpenID Connect, CAS

### Video Platforms

- **Direction**: Bidirectional
- **Data**: Video assets, viewing analytics, transcripts
- **Pattern**: LMS embeds and tracks video via API
- **Protocol**: Platform-specific APIs, LTI

### Analytics Platforms

- **Direction**: LMS → Analytics
- **Data**: IMS Caliper events, custom analytics events
- **Pattern**: LMS produces standardized analytics events
- **Protocol**: IMS Caliper Analytics, custom ETL

## Integration Infrastructure

- **API Gateway**: Central entry point for external system access
- **Event Bus**: Asynchronous event distribution between contexts
- **Webhook Support**: Push notifications to external systems on domain events
- **Batch Sync**: Scheduled synchronization for SIS data, grade exports
- **Idempotency**: All integration endpoints support idempotent operations

## Relationships to Other Topics

- [Context Map](../context-map/) — Maps integration relationships between contexts
- [Anti-Corruption Layer](../anti-corruption-layer/) — Protects domain model from external systems
- [Event-Driven Architecture](../event-driven-architecture/) — Events drive integration
- [LMS Standards](../lms-standards/) — Standards used in integration protocols

## References

- Hohpe, Gregor and Bobby Woolf. "Enterprise Integration Patterns." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13. Addison-Wesley, 2013.
- IMS Global Learning Consortium. "LTI Specification." Version 1.3. 2019.
