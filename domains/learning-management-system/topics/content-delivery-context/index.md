# Content Delivery Context in Learning Management System

## Overview

The Content Delivery bounded context manages the presentation, sequencing, and tracking of learning content including videos, documents, interactive activities, and SCORM/xAPI packages. It handles how learners consume course materials.

## Ubiquitous Language

| Term | Meaning in This Context |
|------|------------------------|
| Content Item | A single piece of learning material (video, document, interactive) |
| Lesson | A structured sequence of content items within a module |
| Content Package | A bundled set of content following a standard (SCORM, xAPI) |
| Player | The rendering engine for a specific content type |
| Bookmark | A learner's saved position within content |
| Sequence | The ordered flow of content items, potentially adaptive |
| Launch | The act of starting a content item for a learner |

## Aggregates

### ContentModule Aggregate

- **Root**: ContentModule (ModuleID + CourseID)
- **Contains**: Lesson, ContentItem
- **Value Objects**: ContentType, Duration, SequenceRule, MediaReference
- **Invariants**:
  - Lessons follow defined sequence order
  - Required content items must be completed before advancing (if sequential)
  - Content must be renderable (valid format and accessible media)

## Key Operations

- `launchContent(StudentID, ContentItemID)` → Initialize content player, record access
- `recordProgress(StudentID, ContentItemID, progressData)` → Track viewing time, interactions, completion
- `getNextContent(StudentID, ModuleID)` → Determine next item based on sequence rules and progress
- `resumeContent(StudentID, ContentItemID)` → Restore learner's bookmark position
- `renderSCORMPackage(packageID, StudentID)` → Launch SCORM player with learner context

## Domain Events Produced

- ContentAccessed, ContentCompleted, LessonCompleted
- BookmarkSaved, SCORMDataCommitted
- xAPIStatementGenerated

## Integration Points

- **Course Catalog Context**: Receives published course structure and content references
- **Progress Context**: Sends completion events for progress tracking
- **Assessment Context**: Content completion may unlock assessments
- **LMS Standards**: SCORM runtime API, xAPI statement generation, LTI tool launches
- **File Storage**: Retrieves media assets and documents
- **Video Hosting**: Streams video content

## Content Types Supported

| Type | Player | Tracking |
|------|--------|----------|
| Video | Video player (streaming) | Watch time, completion percentage |
| Document | Document viewer (PDF, HTML) | Pages viewed, time spent |
| Interactive | HTML5/JavaScript activity | Interactions, score, completion |
| SCORM | SCORM 1.2/2004 runtime | cmi data model elements |
| xAPI | xAPI-enabled content | xAPI statements to LRS |
| LTI | External tool via LTI launch | Grade passback, completion |

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six LMS bounded contexts
- [Course Catalog Context](../course-catalog-context/) — Provides course structure
- [LMS Standards](../lms-standards/) — SCORM, xAPI, LTI implementation details
- [Learner Progress Context](../learner-progress-context/) — Receives content completion events

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- ADL. "SCORM 2004 4th Edition." Advanced Distributed Learning, 2009.
- ADL. "Experience API (xAPI) Specification." Version 1.0.3. 2017.
