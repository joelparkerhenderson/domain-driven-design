# Learning Management System DDD - Ubiquitous Language

Aggregate: A cluster of domain objects treated as a single unit for data consistency, such as a Course with its Modules and Lessons.
Aggregate Root: The single entity through which all access to an aggregate is managed, ensuring invariant enforcement.
Anti-Corruption Layer: A translation boundary that protects a bounded context's domain model from external system concepts.
Assessment: An evaluation instrument used to measure learner knowledge, skills, or competencies, such as a quiz, exam, or assignment.
Assignment: A task or project given to learners that requires submission of work for evaluation and grading.
Attempt: A single instance of a learner taking an assessment, recording answers, score, and completion time.
Bounded Context: A well-defined boundary within which a particular domain model applies and terms have specific meaning.
Caliper: IMS Global analytics standard for measuring and reporting learning activities and outcomes.
Certificate: A formal credential issued to a learner upon successful completion of a course or learning path.
Cohort: A group of learners who progress through a course or program together on the same schedule.
Collaboration: Interaction between learners and instructors through forums, group projects, peer reviews, and messaging.
Completion Status: A value indicating a learner's progress state for a content item or course (not started, in progress, completed, failed).
Content Item: A discrete piece of learning material such as a video, document, interactive exercise, or reading assignment.
Context Map: A visual and conceptual representation of the relationships and interactions between bounded contexts.
Core Domain: The most critical and complex subdomain that provides competitive advantage and requires the most investment.
Course: A structured learning experience with defined objectives, content, assessments, and completion criteria.
Curriculum: An organized set of courses and learning paths designed to achieve broader educational or professional goals.
Discussion Forum: A collaborative space where learners and instructors exchange ideas, ask questions, and discuss course topics.
Domain Event: A record of something significant that happened in the domain, such as StudentEnrolled or CourseCompleted.
Domain Service: An operation that belongs to the domain but does not naturally fit within a single entity or value object.
Enrollment: The formal registration of a learner in a course, granting access to content, assessments, and collaboration features.
Entity: A domain object defined by its unique identity that persists over time, such as Course, Student, or Instructor.
Feedback: Qualitative or quantitative response from an instructor or peer regarding a learner's submitted work.
Generic Subdomain: A subdomain that is not specific to the organization and can use off-the-shelf solutions.
Grade: An immutable value representing the score or mark awarded for an assessment, assignment, or course.
Grading Rubric: A structured set of criteria and performance levels used to evaluate learner submissions consistently.
Group Project: A collaborative assignment where multiple learners work together to produce a shared deliverable.
Instructor: A person responsible for teaching, facilitating, and evaluating learners within a course.
Learning Objective: A specific, measurable statement of what a learner should know or be able to do after completing a course or module.
Learning Path: An ordered sequence of courses and activities designed to guide a learner toward a defined competency or credential.
Lesson: A single instructional unit within a module, containing content items and optional practice activities.
LTI (Learning Tools Interoperability): IMS Global standard for integrating external tools and content into an LMS.
Module: A thematic grouping of lessons within a course, typically covering a distinct topic or learning objective.
Peer Review: An assessment method where learners evaluate each other's work using defined criteria.
Prerequisite: A course or competency that must be completed before a learner can enroll in or access another course.
Progress: A learner's advancement through course content, measured by completed items, assessments taken, and time spent.
Published Language: A well-documented shared data format used for communication between bounded contexts.
Quiz: A short assessment used to check learner understanding of recently covered material.
Repository: A mechanism for encapsulating storage, retrieval, and search behavior for aggregate roots.
Rubric: A scoring guide with criteria and performance levels used to assess learner work.
SCORM (Sharable Content Object Reference Model): A standard for packaging and tracking e-learning content across LMS platforms.
Shared Kernel: A small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together.
Student: A learner enrolled in one or more courses, identified by a unique student ID.
Submission: A learner's completed work submitted for assessment, including the content, timestamp, and metadata.
Supporting Subdomain: A subdomain that is necessary for the business but not a core differentiator.
Syllabus: A document outlining a course's objectives, schedule, assessments, policies, and required materials.
Transcript: An official record of a learner's completed courses, grades, and earned credentials.
Value Object: An immutable domain object defined by its attributes rather than identity, such as Grade, Duration, or CompletionStatus.
Waitlist: A queue of learners waiting to enroll in a course that has reached its maximum capacity.
xAPI (Experience API): A standard for tracking and recording learning experiences across multiple platforms and contexts.
