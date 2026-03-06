# Entities in Learning Management System

## Overview

Entities in an LMS represent core identifiable objects tracked across their lifecycle.

## LMS Entities

### Course

- **Identity**: CourseID (system-generated or catalog code)
- **Attributes**: Title, description, instructor, credit hours, duration, prerequisites, status
- **Lifecycle**: Draft → Published → Active → Archived
- **Context**: Aggregate root in Course Catalog

### Student (Learner)

- **Identity**: StudentID (may map to SIS student ID)
- **Attributes**: Name, email, program, enrollment history, preferences
- **Lifecycle**: Registered → Active → Graduated / Inactive
- **Context**: Referenced across all contexts

### Instructor

- **Identity**: InstructorID
- **Attributes**: Name, credentials, department, courses taught, expertise areas
- **Lifecycle**: Active → (course assignments) → Inactive
- **Context**: Referenced in Catalog, Assessment, Collaboration

### Assessment

- **Identity**: AssessmentID
- **Attributes**: Title, type (quiz/exam/assignment), course reference, questions, time limit, attempt limit
- **Lifecycle**: Draft → Published → Active → Closed → Archived
- **Context**: Aggregate root in Assessment Context

### Certificate

- **Identity**: CertificateID
- **Attributes**: Learner reference, course/path reference, issue date, expiration date, credential data
- **Lifecycle**: Issued → Active → Expired / Revoked
- **Context**: Entity in Learner Progress Context

### LearningPath

- **Identity**: LearningPathID
- **Attributes**: Title, description, ordered courses, completion criteria, target competencies
- **Lifecycle**: Draft → Published → Active → Archived
- **Context**: Aggregate root in Course Catalog

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Entities serve as aggregate roots
- [Value Objects](../value-objects/) — Entities contain value objects
- [Repositories](../repositories/) — Repositories persist entities
- [Ubiquitous Language](../ubiquitous-language/) — Entity names from domain vocabulary

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 5. Addison-Wesley, 2013.
