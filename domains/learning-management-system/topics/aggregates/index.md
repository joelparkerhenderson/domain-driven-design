# Aggregates in Learning Management System

## Overview

Aggregates in an LMS model the core transactional units: courses with their modules, enrollments with their policies, assessments with their questions, and learner records with their completions.

## LMS Aggregates

### Course Aggregate (Catalog)

- **Root**: Course (identified by CourseID)
- **Internal entities**: Module, LearningObjective
- **Value objects**: Duration, CreditHours, Prerequisite, CourseVersion, Syllabus
- **Invariants**: Modules must have sequential ordering; prerequisites must reference published courses; published courses are version-locked
- **Events**: CourseCreated, CoursePublished, CourseUpdated, CourseArchived

### Enrollment Aggregate

- **Root**: Enrollment (identified by EnrollmentID)
- **Value objects**: EnrollmentStatus (enrolled, waitlisted, dropped, completed), EnrollmentDate, CompletionDeadline
- **Invariants**: Cannot enroll beyond course capacity; prerequisites must be satisfied; cannot drop after completion deadline
- **Events**: StudentEnrolled, StudentWaitlisted, StudentDropped, EnrollmentCompleted

### Assessment Aggregate

- **Root**: Assessment (identified by AssessmentID)
- **Internal entities**: Question, GradingRubric
- **Value objects**: Grade, Score, Duration, AttemptLimit, PassingThreshold
- **Invariants**: Assessment must have at least one question; passing threshold ≤ maximum score; graded assessments are immutable
- **Events**: AssessmentCreated, AssessmentPublished, AssessmentGraded

### Assignment Aggregate

- **Root**: Assignment (identified by AssignmentID)
- **Internal entities**: Submission
- **Value objects**: DueDate, Grade, RubricCriteria, Feedback
- **Invariants**: Submissions after due date flagged as late; grade must use rubric criteria; feedback requires grade
- **Events**: AssignmentCreated, AssignmentSubmitted, AssignmentGraded, FeedbackProvided

### LearnerRecord Aggregate (Progress)

- **Root**: LearnerRecord (identified by StudentID + CourseID)
- **Value objects**: CompletionStatus, Progress (percentage), TimeSpent, CompletionDate
- **Invariants**: Completion requires all required modules and assessments passed; progress percentage = completed items / total items
- **Events**: LessonCompleted, ModuleCompleted, CourseCompleted, CertificateIssued

### Discussion Aggregate (Collaboration)

- **Root**: Discussion (identified by DiscussionID)
- **Internal entities**: Thread, Post
- **Value objects**: PostContent, Timestamp
- **Invariants**: Posts must reference valid threads; threads must reference valid discussions; deleted posts are soft-deleted
- **Events**: ThreadCreated, PostCreated, PostReplied

## Aggregate Design Rules

1. **Reference by identity**: Enrollment references Course by CourseID and Student by StudentID
2. **Events for cross-aggregate coordination**: StudentEnrolled triggers LearnerRecord creation
3. **Small aggregates**: Course contains Modules but references Instructor by ID

## Relationships to Other Topics

- [Entities](../entities/) — Aggregate roots are entities
- [Value Objects](../value-objects/) — Aggregates contain value objects
- [Domain Events](../domain-events/) — Aggregates produce events
- [Repositories](../repositories/) — One repository per aggregate root

## References

- Evans, Eric. "Domain-Driven Design." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 10. Addison-Wesley, 2013.
