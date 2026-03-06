# Course Catalog Context in Learning Management System

## Overview

The Course Catalog bounded context manages course definitions, curriculum structure, learning paths, and the publishing lifecycle. It is the authoritative source for what courses exist and how they are organized.

## Ubiquitous Language

| Term | Meaning in This Context |
|------|------------------------|
| Course | A structured learning experience with objectives, modules, and assessments |
| Module | A sequential unit within a course containing lessons and activities |
| Learning Path | An ordered collection of courses leading to a competency or credential |
| Prerequisite | A course or competency required before enrollment |
| Syllabus | The course outline including objectives, schedule, and policies |
| Catalog | The searchable collection of all published courses |
| Version | A snapshot of course content at a point in time |

## Aggregates

### Course Aggregate

- **Root**: Course (CourseID)
- **Contains**: Module, LearningObjective
- **Value Objects**: Duration, CreditHours, Prerequisite, CourseVersion, Syllabus
- **Invariants**:
  - Modules must have sequential ordering
  - Prerequisites must reference published courses
  - Published courses are version-locked (edit creates new draft version)
  - Must have at least one learning objective

### LearningPath Aggregate

- **Root**: LearningPath (LearningPathID)
- **Contains**: PathStep (ordered course references)
- **Value Objects**: CompletionCriteria, TargetCompetency
- **Invariants**:
  - All referenced courses must be published
  - Steps must have sequential ordering
  - Path must have defined completion criteria

## Key Operations

- `createCourse(title, description, objectives)` → Draft course
- `addModule(courseID, module)` → Add module to draft course
- `publishCourse(courseID)` → Validate and publish, creating a locked version
- `archiveCourse(courseID)` → Remove from active catalog
- `createLearningPath(title, courses, criteria)` → Define multi-course path
- `searchCatalog(criteria)` → Full-text search with filters

## Domain Events Produced

- CourseCreated, CoursePublished, CourseUpdated, CourseArchived
- LearningPathCreated, LearningPathPublished

## Integration Points

- **Enrollment Context**: Provides course availability and prerequisites for enrollment checks
- **Content Delivery Context**: Published courses trigger content staging
- **Assessment Context**: Course structure defines assessment placement
- **Progress Context**: Course completion criteria used for progress evaluation

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six LMS bounded contexts
- [Aggregates](../aggregates/) — Course and LearningPath aggregates defined here
- [Enrollment Context](../enrollment-context/) — Consumes catalog data for enrollment
- [Content Delivery Context](../content-delivery-context/) — Delivers published course content

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
