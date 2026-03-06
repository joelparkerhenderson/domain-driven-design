# Context Map in Learning Management System

## Overview

The context map documents relationships between LMS bounded contexts. Courses are defined in the catalog, enrolled through the enrollment context, content is delivered, assessments are taken, progress is tracked, and collaboration occurs throughout.

## Context Relationships

### Course Catalog → Enrollment (Customer-Supplier)

- Catalog is upstream; Enrollment is downstream
- Enrollment depends on course definitions (prerequisites, capacity limits)
- CoursePublished events trigger enrollment availability

### Course Catalog → Content Delivery (Customer-Supplier)

- Catalog defines course structure; Content Delivery renders it
- CourseStructureUpdated events trigger content reorganization

### Enrollment → Learner Progress (Customer-Supplier)

- Enrollment is upstream; Progress is downstream
- StudentEnrolled events create learner progress records
- StudentDropped events archive progress records

### Content Delivery → Learner Progress (Customer-Supplier)

- Content events (LessonCompleted, ModuleCompleted) update progress tracking

### Assessment → Learner Progress (Customer-Supplier)

- Assessment results (AssessmentGraded, AssignmentGraded) update transcripts and completion status

### Course Catalog → Assessment (Customer-Supplier)

- Catalog defines assessment requirements; Assessment context implements them

### Collaboration → Learner Progress (Customer-Supplier)

- Participation metrics from Collaboration feed into progress analytics

### External Systems

- **SIS (Student Information System)** → Enrollment: ACL for student records
- **Content Repositories** → Content Delivery: ACL for SCORM/xAPI packages
- **External Assessment Tools** → Assessment: LTI integration via Open Host Service
- **Analytics Platforms** → Learner Progress: Caliper/xAPI events via Published Language
- **Video Platforms** → Content Delivery: ACL for video hosting APIs

## Integration Summary

| Upstream | Downstream | Pattern | Mechanism |
|----------|-----------|---------|-----------|
| Catalog | Enrollment | Customer-Supplier | Domain Events |
| Catalog | Content Delivery | Customer-Supplier | Domain Events |
| Catalog | Assessment | Customer-Supplier | Domain Events |
| Enrollment | Learner Progress | Customer-Supplier | Domain Events |
| Content Delivery | Learner Progress | Customer-Supplier | Domain Events |
| Assessment | Learner Progress | Customer-Supplier | Domain Events |
| Collaboration | Learner Progress | Customer-Supplier | Domain Events |
| SIS | Enrollment | ACL | API |
| External Tools | Assessment | Open Host (LTI) | LTI Protocol |
| Analytics | Learner Progress | Published Language | xAPI/Caliper |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — Nodes on the context map
- [Integration Patterns](../integration-patterns/) — Detailed pattern descriptions
- [Anti-Corruption Layer](../anti-corruption-layer/) — External system integration
- [Domain Events](../domain-events/) — Primary communication mechanism

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
