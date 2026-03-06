# Ubiquitous Language in Learning Management System

## Overview

Ubiquitous language in an LMS ensures developers and education professionals use the same terms. Education has well-established terminology that the software model should respect.

## One Language per Context

| Term | Catalog | Enrollment | Content | Assessment | Progress |
|------|---------|-----------|---------|-----------|----------|
| Course | Definition with objectives | Capacity and prerequisites | Content package | Assessment plan | Completion record |
| Student | N/A | Enrollee | Learner | Test taker | Learner record |
| Module | Structural unit | N/A | Content container | Assessment group | Progress milestone |

## Language in Code

- Entities: `Course`, `Student`, `Instructor`, `Assignment`, `Certificate`
- Methods: `course.publish()`, `enrollment.enroll()`, `assessment.grade()`, `learner.completeCourse()`
- Events: `CoursePublished`, `StudentEnrolled`, `AssignmentSubmitted`, `CourseCompleted`
- Value objects: `Grade`, `CompletionStatus`, `Duration`, `ContentType`, `LearningObjective`

See [language.md](../../language.md) for the full glossary.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Language developed during strategic design
- [Bounded Contexts](../bounded-contexts/) — Each context has its own language
- [Entities](../entities/) — Named with ubiquitous language
- [Domain Events](../domain-events/) — Named with past-tense domain language

## References

- Evans, Eric. "Domain-Driven Design." Chapter 2. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 1. Addison-Wesley, 2013.
