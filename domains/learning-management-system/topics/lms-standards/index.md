# LMS Standards in Learning Management System

## Overview

LMS standards ensure interoperability between learning platforms, content providers, assessment tools, and analytics systems. These industry standards shape the domain model by defining shared data formats, communication protocols, and integration patterns.

## Key Standards

### SCORM (Sharable Content Object Reference Model)

- **Purpose**: Standardizes content packaging and runtime communication between content and LMS
- **Versions**: SCORM 1.2 (widely adopted), SCORM 2004 (sequencing and navigation)
- **Components**:
  - Content Aggregation Model (CAM): How content is packaged (imsmanifest.xml)
  - Run-Time Environment (RTE): JavaScript API for content-LMS communication
  - Sequencing and Navigation (SN): Rules for content flow (2004 only)
- **Domain Impact**: CompletionStatus, Score, Duration value objects align with SCORM data model elements (cmi.completion_status, cmi.score, cmi.session_time)
- **Publisher**: Advanced Distributed Learning (ADL)

### xAPI (Experience API / Tin Can API)

- **Purpose**: Tracks learning experiences as "Actor-Verb-Object" statements stored in a Learning Record Store (LRS)
- **Key Concepts**:
  - Statement: `{ actor, verb, object, result, context, timestamp }`
  - Actor: The learner (identified by email, account, or OpenID)
  - Verb: What the learner did (completed, passed, attempted, answered)
  - Object: What was acted upon (course, assessment, content item)
- **Domain Impact**: Domain events map naturally to xAPI statements; ContentAccessed → "experienced", AssessmentGraded → "scored", CourseCompleted → "completed"
- **Publisher**: ADL / IEEE

### LTI (Learning Tools Interoperability)

- **Purpose**: Enables external tools to integrate seamlessly into the LMS
- **Versions**: LTI 1.1 (basic launch), LTI 1.3 (security improvements), LTI Advantage (deep linking, grade passback, names and roles)
- **Key Concepts**:
  - Tool Consumer: The LMS that launches external tools
  - Tool Provider: The external application being launched
  - Deep Linking: Browse and select content from external tools
  - Assignment and Grade Services: Pass grades back to LMS gradebook
- **Domain Impact**: External tools appear as ContentItems; grade passback integrates with Assessment context
- **Publisher**: IMS Global / 1EdTech

### IMS Caliper Analytics

- **Purpose**: Standardizes learning analytics event data
- **Key Concepts**:
  - Sensor API: Generates standardized metric profiles
  - Events: Structured learning events (NavigationEvent, AssessmentEvent, GradeEvent)
  - Profiles: Predefined event patterns for common scenarios (reading, assessment, grading)
- **Domain Impact**: Domain events align with Caliper event profiles for analytics export
- **Publisher**: IMS Global / 1EdTech

### QTI (Question and Test Interoperability)

- **Purpose**: Standardizes assessment content for portability between systems
- **Key Concepts**:
  - AssessmentItem: A single question with response processing rules
  - AssessmentTest: A collection of items with section structure
  - Interaction Types: choiceInteraction, textEntryInteraction, etc.
- **Domain Impact**: Assessment aggregate can import/export QTI format; Question entity maps to QTI AssessmentItem
- **Publisher**: IMS Global / 1EdTech

### IMS Common Cartridge / Thin Common Cartridge

- **Purpose**: Packages complete course content (content, assessments, discussions) for portability
- **Domain Impact**: Course aggregate import/export in standard format
- **Publisher**: IMS Global / 1EdTech

## Standards Mapping to Domain Model

| Standard | Domain Concept | Mapping |
|----------|---------------|---------|
| SCORM cmi.completion_status | CompletionStatus | completed, incomplete, not attempted |
| SCORM cmi.score.scaled | Score | Percentage as 0-1 decimal |
| xAPI verb | Domain Event | completed → CourseCompleted, scored → AssessmentGraded |
| LTI launch | ContentItem | External tool as content within module |
| LTI AGS | Grade | Grade passback from external tool |
| QTI assessmentItem | Question | Question entity with response processing |
| Caliper event | Domain Event | NavigationEvent → ContentAccessed |

## Relationships to Other Topics

- [Content Delivery Context](../content-delivery-context/) — SCORM/xAPI content rendering
- [Assessment Context](../assessment-context/) — QTI assessment interoperability
- [Integration Patterns](../integration-patterns/) — Standards define integration protocols
- [Domain Events](../domain-events/) — Events map to xAPI statements and Caliper events

## References

- ADL. "SCORM 2004 4th Edition." Advanced Distributed Learning, 2009.
- ADL. "Experience API (xAPI) Specification." Version 1.0.3. 2017.
- IMS Global. "Learning Tools Interoperability (LTI) Specification." Version 1.3. 2019.
- IMS Global. "Caliper Analytics Specification." Version 1.2. 2019.
- IMS Global. "QTI Specification." Version 3.0. 2020.
