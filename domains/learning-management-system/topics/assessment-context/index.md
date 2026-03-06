# Assessment Context in Learning Management System

## Overview

The Assessment bounded context manages the creation, administration, grading, and analysis of quizzes, exams, assignments, and peer reviews. It handles both automated and manual grading workflows.

## Ubiquitous Language

| Term | Meaning in This Context |
|------|------------------------|
| Assessment | A formal evaluation of student learning (quiz, exam, assignment) |
| Question | A single item within an assessment requiring a response |
| Attempt | A student's single try at completing an assessment |
| Rubric | A structured grading guide with criteria and performance levels |
| Grade | The evaluated result of an assessment attempt |
| Submission | A student's completed work for an assignment |
| Question Bank | A pool of questions from which assessments can be drawn |
| Feedback | Instructor comments on a graded submission |

## Aggregates

### Assessment Aggregate

- **Root**: Assessment (AssessmentID)
- **Contains**: Question, GradingRubric
- **Value Objects**: Grade, Score, Duration (time limit), AttemptLimit, PassingThreshold
- **Invariants**:
  - Assessment must have at least one question
  - Passing threshold must not exceed maximum score
  - Graded assessments are immutable (scores cannot be retroactively changed without formal process)
  - Time limit must be positive if set

### Assignment Aggregate

- **Root**: Assignment (AssignmentID)
- **Contains**: Submission
- **Value Objects**: DueDate, Grade, RubricCriteria, Feedback
- **Invariants**:
  - Submissions after due date are flagged as late
  - Grade must use rubric criteria if rubric is defined
  - Feedback requires a grade (cannot provide feedback without grading)
  - Only one active submission per student (resubmission replaces previous)

## Key Operations

- `createAssessment(title, type, questions, settings)` → Draft assessment with questions
- `publishAssessment(AssessmentID)` → Make assessment available to students
- `startAttempt(StudentID, AssessmentID)` → Begin timed assessment attempt
- `submitAttempt(StudentID, AssessmentID, responses)` → Submit responses for grading
- `autoGrade(AttemptID)` → Grade objective questions automatically
- `manualGrade(AttemptID, instructorGrades)` → Instructor grades subjective questions
- `submitAssignment(StudentID, AssignmentID, files)` → Submit assignment work
- `gradeWithRubric(SubmissionID, rubricScores, feedback)` → Grade using rubric criteria

## Question Types

| Type | Auto-Gradable | Description |
|------|--------------|-------------|
| Multiple Choice | Yes | Single correct answer from options |
| Multiple Select | Yes | Multiple correct answers from options |
| True/False | Yes | Binary choice |
| Fill-in-the-Blank | Partial | Exact match or regex pattern |
| Short Answer | No | Free-text response |
| Essay | No | Extended written response |
| File Upload | No | Document or artifact submission |
| Matching | Yes | Pair items from two lists |

## Domain Events Produced

- AssessmentCreated, AssessmentPublished
- AttemptStarted, AssessmentSubmitted, AssessmentGraded
- AssignmentSubmitted, AssignmentGraded, FeedbackProvided

## Integration Points

- **Content Delivery Context**: Assessment placement within course modules
- **Progress Context**: Grade events update learner records and completion status
- **Enrollment Context**: Assessment availability tied to enrollment status
- **Analytics**: Assessment results feed learning analytics and item analysis
- **QTI**: Import/export assessments in QTI standard format

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six LMS bounded contexts
- [Aggregates](../aggregates/) — Assessment and Assignment aggregates defined here
- [Learner Progress Context](../learner-progress-context/) — Receives grading events
- [LMS Standards](../lms-standards/) — QTI for assessment interoperability

## References

- Evans, Eric. "Domain-Driven Design." Chapter 14. Addison-Wesley, 2003.
- IMS Global. "QTI (Question and Test Interoperability) Specification." Version 3.0. 2020.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
