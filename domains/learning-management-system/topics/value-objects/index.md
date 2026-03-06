# Value Objects in Learning Management System

## Overview

Value objects in an LMS represent grades, durations, completion statuses, content types, and other descriptive data.

## LMS Value Objects

### Grade

- **Attributes**: Score (numeric), maximum score, percentage, letter grade (optional), pass/fail
- **Behavior**: `isPassing(threshold)` returns boolean
- **Usage**: Assessment results, assignment grades, course final grades
- **Example**: `Grade(85, 100, 85.0, "B", true)`

### CompletionStatus

- **Attributes**: Status (not_started, in_progress, completed, failed), completion date, completion percentage
- **Usage**: Lesson, module, and course completion tracking
- **Example**: `CompletionStatus("completed", "2026-03-06", 100.0)`

### Duration

- **Attributes**: Amount, unit (minutes, hours, days, weeks)
- **Usage**: Course duration, assessment time limits, content viewing time
- **Example**: `Duration(90, "minutes")`

### ContentType

- **Attributes**: Type (video, document, interactive, quiz, discussion, assignment), MIME type (optional)
- **Usage**: Content item classification, rendering decisions

### LearningObjective

- **Attributes**: Description, Bloom's taxonomy level (remember, understand, apply, analyze, evaluate, create), measurable outcome
- **Usage**: Course and module learning goals

### Prerequisite

- **Attributes**: Required course ID, minimum grade (optional), required completion status
- **Usage**: Enrollment eligibility checks

### RubricCriteria

- **Attributes**: Criterion name, description, performance levels (with point values), weight
- **Usage**: Assignment and peer review grading

### Score

- **Attributes**: Points earned, points possible
- **Behavior**: `percentage()` returns earned/possible × 100
- **Usage**: Quiz scores, exam scores, activity scores

### DateRange

- **Attributes**: Start date, end date
- **Usage**: Enrollment periods, course availability windows, assignment due windows

### CreditHours

- **Attributes**: Credits (decimal), credit type (academic, continuing education, professional development)
- **Usage**: Course credit assignment, transcript records

## Relationships to Other Topics

- [Entities](../entities/) — Entities contain value objects
- [Aggregates](../aggregates/) — Value objects within aggregates
- [LMS Standards](../lms-standards/) — SCORM completion statuses, xAPI verbs

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6. Addison-Wesley, 2013.
