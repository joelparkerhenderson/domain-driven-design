# Anti-Corruption Layer in Learning Management System

## Overview

ACLs protect LMS bounded contexts from external system concepts. LMS platforms integrate with student information systems, content repositories, external assessment tools, video platforms, and analytics services.

## LMS ACL Examples

### Student Information System (SIS) → Enrollment Context

SIS systems (Banner, PeopleSoft, Workday Student) have their own student and course models. The ACL:

- Translates SIS student records into domain Student entities
- Maps SIS course sections to internal Enrollment offerings
- Converts SIS enrollment statuses to domain EnrollmentStatus
- Handles term/semester structures that may differ from LMS periods

### SCORM/xAPI Content → Content Delivery Context

External content packages use SCORM or xAPI standards. The ACL:

- Parses SCORM manifests into domain ContentModule and Lesson structures
- Translates SCORM completion data into domain CompletionStatus
- Converts xAPI statements into domain LearnerActivity events
- Maps external content identifiers to internal ContentItem references

### External Assessment Tools → Assessment Context (via LTI)

Tools like Turnitin, Respondus, or Proctorio integrate via LTI. The ACL:

- Translates LTI grade passback into domain Grade value objects
- Maps external tool submission data to internal Submission entities
- Converts tool-specific integrity scores to domain IntegrityReport values

### Video Platforms → Content Delivery Context

Platforms like Kaltura, Panopto, or Vimeo provide video hosting. The ACL:

- Translates video platform metadata into domain MediaAsset value objects
- Maps platform-specific viewing analytics to domain ViewingProgress events
- Converts embed codes to internal ContentItem references

### Analytics Platforms → Learner Progress Context

Learning analytics tools consume activity data. The ACL:

- Translates internal progress events into Caliper or xAPI statements
- Maps internal completion and grade data to analytics-standard formats
- Converts analytics insights back into domain recommendations

## Relationships to Other Topics

- [Context Map](../context-map/) — ACLs appear on the context map
- [Integration Patterns](../integration-patterns/) — ACL is one integration pattern
- [LMS Standards](../lms-standards/) — SCORM, xAPI, LTI define external formats

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 3. Addison-Wesley, 2013.
- Hohpe, Gregor & Woolf, Bobby. "Enterprise Integration Patterns." Addison-Wesley, 2003.
