# Bounded Contexts in Learning Management System

## Overview

Bounded contexts define boundaries within which LMS domain models are consistent. A "Course" means different things to a curriculum designer (learning objectives, structure), an enrollment officer (capacity, prerequisites), and a content author (modules, lessons, media).

## The Six LMS Bounded Contexts

### Course Catalog Context

- **Focus**: Course definitions, curriculum structure, learning paths, prerequisites, syllabus, learning objectives, course versioning
- **Aggregate Roots**: Course, LearningPath, Curriculum
- **Language**: Course, Module, Learning Objective, Prerequisite, Syllabus, Learning Path, Curriculum, Credit Hours

### Enrollment Context

- **Focus**: Student registration, enrollment lifecycle, waitlists, enrollment policies, cohort management, capacity management
- **Aggregate Roots**: Enrollment, EnrollmentPolicy
- **Language**: Enrollment, Registration, Waitlist, Cohort, Capacity, Drop, Withdrawal, Enrollment Period

### Content Delivery Context

- **Focus**: Lessons, content items (video, text, interactive), content sequencing, SCORM packages, adaptive content, content versioning
- **Aggregate Roots**: ContentModule, Lesson
- **Language**: Lesson, Content Item, Module, SCORM Package, Sequencing, Adaptive Content, Media Asset

### Assessment Context

- **Focus**: Quizzes, exams, assignments, grading rubrics, question banks, feedback, academic integrity
- **Aggregate Roots**: Assessment, Assignment, QuestionBank
- **Language**: Quiz, Exam, Assignment, Question, Rubric, Grade, Submission, Attempt, Feedback, Proctoring

### Learner Progress Context

- **Focus**: Progress tracking, completion status, certificates, transcripts, learning analytics, competency tracking
- **Aggregate Roots**: LearnerRecord, Certificate, Transcript
- **Language**: Progress, Completion, Certificate, Transcript, Competency, Badge, Credit, GPA

### Collaboration Context

- **Focus**: Discussion forums, group projects, peer reviews, messaging, announcements, live sessions
- **Aggregate Roots**: Discussion, GroupProject, PeerReview
- **Language**: Forum, Thread, Post, Group, Peer Review, Announcement, Message, Live Session

## Boundary Enforcement

Each context maintains its own model. A "Course" in the Catalog is a CourseDefinition (objectives, structure, prerequisites). In Content Delivery, it is a ContentPackage (modules, lessons, media). In Assessment, it is an AssessmentPlan (quizzes, exams, grading scheme).

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Bounded contexts result from strategic design
- [Context Map](../context-map/) — Shows how contexts relate
- [Ubiquitous Language](../ubiquitous-language/) — Each context has its own language

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
