# Repositories in Learning Management System

## Overview

Repositories in an LMS provide persistence abstractions for aggregate roots, encapsulating storage details and enabling domain-focused data access for courses, enrollments, assessments, and learner records.

## LMS Repositories

### CourseRepository

- **Aggregate Root**: Course
- **Key Operations**: `findById(CourseID)`, `findByStatus(status)`, `findByInstructor(InstructorID)`, `searchByCriteria(keywords, category, level)`, `save(Course)`, `findPublished()`
- **Query Patterns**: Full-text search on title and description, filter by category, level, prerequisites
- **Notes**: Supports versioning; archived courses remain queryable

### EnrollmentRepository

- **Aggregate Root**: Enrollment
- **Key Operations**: `findById(EnrollmentID)`, `findByStudent(StudentID)`, `findByCourse(CourseID)`, `findActiveByStudent(StudentID)`, `save(Enrollment)`, `countByCourse(CourseID)`
- **Query Patterns**: Active enrollments per student, enrollment counts for capacity checks, waitlist ordering
- **Notes**: High-read volume; supports pagination and filtering by status

### AssessmentRepository

- **Aggregate Root**: Assessment
- **Key Operations**: `findById(AssessmentID)`, `findByCourse(CourseID)`, `findByModule(ModuleID)`, `findPublished(CourseID)`, `save(Assessment)`
- **Query Patterns**: Assessments by course and module, published assessments for student view
- **Notes**: Assessment content may be large (questions, rubrics); consider lazy loading

### AssignmentRepository

- **Aggregate Root**: Assignment
- **Key Operations**: `findById(AssignmentID)`, `findByCourse(CourseID)`, `findPendingGrading(InstructorID)`, `findByStudentAndCourse(StudentID, CourseID)`, `save(Assignment)`
- **Query Patterns**: Pending submissions for instructor grading queue, student submission history
- **Notes**: Submission attachments stored separately in file storage

### LearnerRecordRepository

- **Aggregate Root**: LearnerRecord
- **Key Operations**: `findByStudentAndCourse(StudentID, CourseID)`, `findByStudent(StudentID)`, `findCompletedByStudent(StudentID)`, `save(LearnerRecord)`
- **Query Patterns**: Student transcript (all completed courses), progress dashboard (active courses), completion rates
- **Notes**: Read-heavy; supports CQRS read models for dashboards and analytics

### DiscussionRepository

- **Aggregate Root**: Discussion
- **Key Operations**: `findById(DiscussionID)`, `findByCourse(CourseID)`, `findByModule(ModuleID)`, `findRecentThreads(DiscussionID, limit)`, `save(Discussion)`
- **Query Patterns**: Threaded discussion views, recent activity, unread posts per student
- **Notes**: Pagination critical for large discussions; soft-deleted posts retained

### StudentRepository

- **Entity**: Student
- **Key Operations**: `findById(StudentID)`, `findByEmail(email)`, `findByProgram(programID)`, `search(criteria)`, `save(Student)`
- **Notes**: Often backed by external SIS; may be read-only or sync-based

### InstructorRepository

- **Entity**: Instructor
- **Key Operations**: `findById(InstructorID)`, `findByDepartment(department)`, `findByCourse(CourseID)`, `save(Instructor)`
- **Notes**: May integrate with HR or identity systems

## Repository Design Principles

1. **One repository per aggregate root**: CourseRepository for Course aggregate, not separate ModuleRepository
2. **Domain-oriented queries**: Methods use domain language, not SQL concepts
3. **Specification pattern**: Complex queries use composable specifications (e.g., `CourseSearchSpecification`)
4. **CQRS read models**: Separate read-optimized projections for dashboards, analytics, and reporting

## Relationships to Other Topics

- [Aggregates](../aggregates/) — One repository per aggregate root
- [Entities](../entities/) — Repositories persist and retrieve entities
- [Domain Services](../domain-services/) — Services use repositories for data access
- [Integration Patterns](../integration-patterns/) — Repository implementations may integrate with external systems

## References

- Evans, Eric. "Domain-Driven Design." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12. Addison-Wesley, 2013.
