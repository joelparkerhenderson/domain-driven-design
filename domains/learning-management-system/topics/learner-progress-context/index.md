# Learner Progress Context in Learning Management System

## Overview

The Learner Progress bounded context tracks student advancement through courses, modules, and learning paths. It maintains completion records, calculates progress, issues certificates, and generates transcripts.

## Ubiquitous Language

| Term | Meaning in This Context |
|------|------------------------|
| Learner Record | The comprehensive record of a student's progress in a course |
| Completion Status | Whether a learner has completed a learning item (not started, in progress, completed, failed) |
| Progress | The percentage of completed items relative to total required items |
| Transcript | An official record of all courses completed with grades and credits |
| Certificate | A verifiable credential issued upon completion |
| Milestone | A significant progress point (module completion, assessment passed) |
| Time Spent | Cumulative time a learner has engaged with content |

## Aggregates

### LearnerRecord Aggregate

- **Root**: LearnerRecord (StudentID + CourseID)
- **Value Objects**: CompletionStatus, Progress (percentage), TimeSpent, CompletionDate, FinalGrade
- **Invariants**:
  - Completion requires all required modules completed and assessments passed at minimum grade
  - Progress percentage = completed required items / total required items
  - CompletionDate set only when status transitions to completed
  - FinalGrade calculated from weighted assessment grades per course grading policy

### Certificate Entity

- **Identity**: CertificateID
- **Attributes**: StudentID, CourseID or LearningPathID, issue date, expiration date, credential data, verification URL
- **Lifecycle**: Issued → Active → Expired / Revoked
- **Invariants**: Can only be issued when course or path completion criteria are fully met

## Key Operations

- `recordContentCompletion(StudentID, ContentItemID, completionData)` → Update progress for content
- `recordAssessmentResult(StudentID, AssessmentID, grade)` → Update progress for assessment
- `evaluateModuleCompletion(StudentID, ModuleID)` → Check if module requirements met
- `evaluateCourseCompletion(StudentID, CourseID)` → Check if course requirements met
- `issueCertificate(StudentID, CourseID)` → Generate certificate upon completion
- `generateTranscript(StudentID)` → Compile all completions, grades, and credits
- `getProgressDashboard(StudentID)` → Return active enrollments with progress data

## Domain Events Consumed

- ContentCompleted (from Content Delivery)
- AssessmentGraded (from Assessment)
- StudentEnrolled (from Enrollment)
- StudentDropped (from Enrollment)

## Domain Events Produced

- LessonCompleted, ModuleCompleted, CourseCompleted
- CertificateIssued, CertificateExpired
- MilestoneReached

## Integration Points

- **Assessment Context**: Receives grade events to update completion tracking
- **Content Delivery Context**: Receives content completion events
- **Enrollment Context**: Receives enrollment and drop events; sends completion status
- **Analytics**: Provides progress data for learning analytics dashboards
- **External Systems**: Exports completion data as xAPI statements, SCORM data, or Open Badges

## Progress Calculation

```
Module Progress = (completed required lessons + passed required assessments) / total required items
Course Progress = weighted average of module progress values
Path Progress   = completed required courses / total required courses
```

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six LMS bounded contexts
- [Assessment Context](../assessment-context/) — Provides grading events
- [Content Delivery Context](../content-delivery-context/) — Provides completion events
- [Enrollment Context](../enrollment-context/) — Provides enrollment events
- [LMS Standards](../lms-standards/) — xAPI, SCORM completion reporting

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
