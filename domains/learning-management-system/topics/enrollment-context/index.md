# Enrollment Context in Learning Management System

## Overview

The Enrollment bounded context manages student registration, waitlists, cohort assignment, capacity management, and drop/withdrawal processing. It serves as the gateway between students and courses.

## Ubiquitous Language

| Term | Meaning in This Context |
|------|------------------------|
| Enrollment | A student's active registration in a course |
| Waitlist | An ordered queue of students awaiting capacity |
| Cohort | A group of students progressing through a course together |
| Capacity | The maximum number of students allowed in a course section |
| Drop | Student-initiated withdrawal before a deadline |
| Withdrawal | Administrative or late removal from a course |
| Registration Period | The window during which enrollment is open |

## Aggregates

### Enrollment Aggregate

- **Root**: Enrollment (EnrollmentID)
- **Value Objects**: EnrollmentStatus (enrolled, waitlisted, dropped, completed), EnrollmentDate, CompletionDeadline, DropDeadline
- **Invariants**:
  - Cannot enroll beyond course capacity
  - Prerequisites must be satisfied before enrollment
  - Cannot drop after completion deadline
  - Waitlist position is first-come, first-served
  - Cannot enroll in same course twice simultaneously

## Key Operations

- `enroll(StudentID, CourseID)` → Check prerequisites, capacity; create enrollment or waitlist
- `dropEnrollment(EnrollmentID, reason)` → Process drop with deadline validation
- `processWaitlist(CourseID)` → Promote next waitlisted student when capacity opens
- `transferSection(EnrollmentID, newSectionID)` → Move student between sections
- `checkEligibility(StudentID, CourseID)` → Return prerequisite and capacity status

## Domain Events Produced

- StudentEnrolled, StudentWaitlisted, StudentDropped
- EnrollmentCompleted, WaitlistPromoted
- CohortAssigned

## Integration Points

- **Course Catalog Context**: Reads course prerequisites, capacity, and availability
- **Progress Context**: Reads completion history for prerequisite validation; receives enrollment status updates
- **Notifications**: Triggers enrollment confirmation, waitlist updates, deadline reminders
- **Financial Systems**: Drop processing may trigger refund workflows

## Policies

- **Prerequisite Policy**: All required courses must show CompletionStatus = completed with minimum grade
- **Capacity Policy**: Enrollment count must not exceed section capacity; overflow goes to waitlist
- **Drop Policy**: Drops before deadline receive full withdrawal; late drops may receive incomplete grade
- **Waitlist Policy**: Students promoted in order; promotion expires if not confirmed within 48 hours

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six LMS bounded contexts
- [Course Catalog Context](../course-catalog-context/) — Provides course data for enrollment
- [Learner Progress Context](../learner-progress-context/) — Receives enrollment events
- [Domain Events](../domain-events/) — Enrollment events trigger cross-context workflows

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
