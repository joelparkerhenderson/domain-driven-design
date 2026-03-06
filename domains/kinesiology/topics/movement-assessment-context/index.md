# Movement Assessment Context - Kinesiology Domain

## Overview

The Movement Assessment Context is the foundational bounded context in the kinesiology domain. It encompasses the systematic evaluation of human movement quality, joint function, muscular strength, neuromuscular control, and overall physical capacity. This context serves as the primary data-gathering function in the kinesiology system, producing the clinical findings that inform exercise prescription, rehabilitation planning, injury prevention, and performance optimization.

Movement assessment is classified as a core subdomain because the quality and sophistication of evaluation directly determines the value of every downstream decision. A kinesiology system that cannot accurately capture and interpret assessment findings fails at its most fundamental purpose.

## Key Concepts

Gait analysis evaluates locomotion patterns to identify deviations from normal walking or running mechanics. It involves observational analysis, temporal-spatial parameter measurement, and potentially instrumented data collection. A complete gait analysis produces stride parameters, joint kinematics during the gait cycle, and identification of compensatory strategies.

Biomechanical evaluation assesses the forces, torques, and movement patterns acting on the musculoskeletal system during functional activities. This includes posture analysis, joint mechanics assessment, and movement pattern evaluation under loaded and unloaded conditions.

Range of motion assessment measures the available arc of movement at each joint, documenting both active range (produced by the individual's own muscular effort) and passive range (produced by an external force). Measurements are recorded in degrees and compared against established norms.

Functional movement screening uses standardized test batteries to evaluate fundamental movement quality. The Functional Movement Screen (FMS) is a widely used battery that assesses seven movement patterns to identify compensatory strategies, asymmetries, and limitations that may predispose an individual to injury.

Manual muscle testing grades the strength of individual muscles or muscle groups using the Medical Research Council scale, providing information about neuromuscular function and identifying strength deficits.

## Aggregate Design

The MovementAssessment aggregate is the primary aggregate root in this context. It represents a complete evaluation session and enforces consistency across all tests performed during that session. The aggregate ensures that all individual test results are associated with the correct subject, that assessment status follows a valid lifecycle, and that composite scores are recalculated when individual test results change.

The GaitAnalysisSession aggregate manages a focused gait evaluation, including multiple trials, temporal-spatial parameter calculations, and deviation analysis. When gait analysis is performed as part of a comprehensive assessment, the MovementAssessment aggregate references the GaitAnalysisSession by identity.

## Entities

The Subject entity represents the individual being assessed. It carries a persistent identity that enables longitudinal tracking across multiple assessments over time.

The Evaluator entity represents the clinician performing the assessment, carrying credentials and certification records relevant to the assessment types they are authorized to perform.

The GaitAnalysis entity represents a single gait evaluation, tracking analysis conditions, collected trials, and derived parameters.

The FunctionalMovementScreen entity represents a single administration of the FMS battery, tracking individual test scores, clearing test results, and composite scores.

## Value Objects

JointAngle captures a measured angle at a specific joint in a specific plane of motion, validated against anatomically plausible ranges.

MuscleGrade represents a manual muscle test result on the standard 0-to-5 scale with plus/minus modifiers.

MovementScore captures scored outcomes from standardized movement tests with validation against the scoring criteria of the specific test.

GaitParameter represents derived gait measurements such as stride length, cadence, stance phase percentage, and gait speed.

## Domain Events

AssessmentCompleted is published when an assessment session is finalized, carrying the assessment identifier, subject identifier, and summary of key findings for consumption by downstream contexts.

MovementDeficitIdentified is published when the assessment identifies a significant movement deficit, enabling the Injury Prevention Context to update risk profiles.

ReassessmentScheduled is published when a follow-up assessment is indicated based on current findings.

## Domain Services

ComprehensiveAssessmentService coordinates the assembly of a complete assessment from multiple individual test batteries and derives composite clinical interpretations.

NormativeComparisonService compares assessment results against age and activity-appropriate normative databases to provide percentile rankings.

AssessmentScoringService applies the specific scoring algorithms for standardized assessment batteries.

## Integration Points

The Movement Assessment Context is upstream to Exercise Prescription, Rehabilitation Planning, and Injury Prevention through customer-supplier relationships. It consumes enrichment data from the Performance Analysis Context through a conformist relationship. It publishes assessment findings using a published language contract for consumption by the Injury Prevention Context.

## Clinical Standards

Assessment protocols align with standards published by the American College of Sports Medicine (ACSM), the National Athletic Trainers' Association (NATA), and the American Physical Therapy Association (APTA). The Functional Movement Screen follows the protocols established by Cook et al. Manual muscle testing follows the Medical Research Council grading system.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Cook, Gray, et al. "Pre-Participation Screening: The Use of Fundamental Movements as an Assessment of Function." North American Journal of Sports Physical Therapy, 2006.
- Neumann, Donald A. Kinesiology of the Musculoskeletal System. 3rd ed. Elsevier, 2017.
- Kendall, Florence P., et al. Muscles: Testing and Function with Posture and Pain. 5th ed. Lippincott Williams & Wilkins, 2005.
