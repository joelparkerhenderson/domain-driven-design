# Domain Events in Learning Management System

## Overview

Domain events in an LMS capture significant occurrences such as enrollments, completions, grade postings, and content interactions that trigger downstream processing across bounded contexts.

## LMS Domain Events

### CoursePublished

- **Trigger**: Instructor or admin publishes a course
- **Data**: CourseID, title, instructor, publish date, version
- **Consumers**: Enrollment (opens registration), Content Delivery (stages content), Catalog (updates listing)

### StudentEnrolled

- **Trigger**: Student successfully enrolls in a course
- **Data**: StudentID, CourseID, enrollment date, cohort (optional)
- **Consumers**: Progress (creates LearnerRecord), Content Delivery (provisions access), Notifications (sends confirmation)

### StudentDropped

- **Trigger**: Student drops or is administratively removed
- **Data**: StudentID, CourseID, drop date, reason
- **Consumers**: Progress (finalizes record), Enrollment (frees capacity), Financial (processes refund if applicable)

### AssessmentSubmitted

- **Trigger**: Student submits an assessment attempt
- **Data**: StudentID, AssessmentID, submission timestamp, attempt number
- **Consumers**: Assessment (queues for grading), Progress (updates activity log)

### AssessmentGraded

- **Trigger**: Assessment is graded (auto or manual)
- **Data**: StudentID, AssessmentID, Grade, attempt number, grader (if manual)
- **Consumers**: Progress (updates completion), Notifications (sends grade alert), Analytics (updates performance data)

### AssignmentSubmitted

- **Trigger**: Student submits an assignment
- **Data**: StudentID, AssignmentID, submission timestamp, file references, late flag
- **Consumers**: Assessment (queues for review), Progress (updates activity), Notifications (alerts instructor)

### LessonCompleted

- **Trigger**: Student completes a lesson (content viewed, activity finished)
- **Data**: StudentID, LessonID, ModuleID, completion timestamp, time spent
- **Consumers**: Progress (updates module progress), Analytics (tracks engagement), Adaptive Learning (adjusts path)

### ModuleCompleted

- **Trigger**: All required lessons and assessments in a module are completed
- **Data**: StudentID, ModuleID, CourseID, completion date, module grade
- **Consumers**: Progress (updates course progress), Notifications (sends milestone alert)

### CourseCompleted

- **Trigger**: Student meets all course completion criteria
- **Data**: StudentID, CourseID, completion date, final grade, credit hours earned
- **Consumers**: Progress (issues certificate), Enrollment (updates status), Transcript (records completion), Analytics

### CertificateIssued

- **Trigger**: Student earns a certificate upon course or path completion
- **Data**: CertificateID, StudentID, CourseID or LearningPathID, issue date, credential data
- **Consumers**: Learner Profile (updates credentials), Notifications (sends certificate), External Systems (badges)

### ContentAccessed

- **Trigger**: Student opens or interacts with content
- **Data**: StudentID, ContentID, access timestamp, content type, duration
- **Consumers**: Analytics (tracks engagement), Adaptive Learning (adjusts recommendations), Progress (records activity)

### DiscussionPostCreated

- **Trigger**: Student or instructor creates a discussion post
- **Data**: PostID, ThreadID, DiscussionID, AuthorID, timestamp
- **Consumers**: Notifications (alerts subscribers), Collaboration (updates thread), Analytics (tracks participation)

## Event Flow Patterns

1. **Enrollment → Progress**: StudentEnrolled creates LearnerRecord
2. **Content → Assessment → Progress**: ContentAccessed → AssessmentSubmitted → AssessmentGraded → ModuleCompleted → CourseCompleted
3. **Completion → Certification**: CourseCompleted → CertificateIssued
4. **xAPI Integration**: Domain events map to xAPI statements for external LRS

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Aggregates produce domain events
- [Event-Driven Architecture](../event-driven-architecture/) — Events enable loose coupling
- [Integration Patterns](../integration-patterns/) — Events cross context boundaries
- [LMS Standards](../lms-standards/) — Events map to xAPI statements and SCORM data model

## References

- Evans, Eric. "Domain-Driven Design." Chapter 8. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 8. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 9. O'Reilly, 2021.
