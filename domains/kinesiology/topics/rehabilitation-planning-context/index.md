# Rehabilitation Planning Context - Kinesiology Domain

## Overview

The Rehabilitation Planning Context manages clinical recovery from injury or surgery. While it shares surface similarities with the Exercise Prescription Context, it introduces distinct concepts around tissue healing biology, clinical milestones, graduated return-to-activity protocols, and therapeutic exercise that operates within healing constraints. This context governs the complete recovery trajectory from initial injury through full functional restoration and return to activity.

Rehabilitation planning is classified as a supporting subdomain. It requires specialized clinical knowledge but follows well-established rehabilitation science frameworks. The domain model must be clinically sound and evidence-based, drawing on established tissue healing timelines and recovery protocols.

## Key Concepts

Post-injury protocols define the structured progression of therapeutic interventions following specific injury types. Protocols are phase-based, with each phase aligned to the biological stage of tissue healing. The inflammatory phase (days 0 to 7) prioritizes pain management, edema control, and protected movement. The proliferative phase (days 7 to 21) introduces progressive loading and functional exercise. The remodeling phase (day 21 onward) advances toward full functional restoration and sport-specific preparation.

Return-to-play criteria are objective, measurable benchmarks that must be satisfied before an individual is cleared for full activity participation. Criteria typically include achieving specified range of motion thresholds, passing strength symmetry tests (such as limb symmetry index greater than 90 percent), completing functional test batteries (hop tests, agility tests), and demonstrating sport-specific skills without pain or apprehension.

Therapeutic exercise is structured physical activity prescribed specifically to restore function, reduce impairment, and promote tissue healing. Unlike general exercise prescription, therapeutic exercise operates within healing constraints, respecting tissue tolerance and clinical precautions. Dosage is guided by tissue response (pain, swelling, function) rather than performance optimization.

Weight-bearing status defines the amount of body weight an individual is permitted to bear through an injured limb, progressing from non-weight-bearing through partial weight-bearing to full weight-bearing as healing permits.

Clinical milestones are specific recovery benchmarks that serve as gateways between rehabilitation phases. Examples include achieving 90 degrees of knee flexion after anterior cruciate ligament reconstruction, demonstrating single-leg stance for 30 seconds, or completing a walk-jog progression without pain.

## Aggregate Design

The RehabilitationPlan aggregate is the primary aggregate root. It governs the entire recovery trajectory and enforces consistency across phases, milestones, and therapeutic exercise sets. The aggregate ensures that phase transitions only occur when mandatory milestones are achieved, that therapeutic exercises are appropriate for the current healing phase, and that return-to-play clearance requires all mandatory criteria to be satisfied.

Key invariants include: rehabilitation phases must progress in sequence (no skipping phases), phase advancement requires achievement of all mandatory milestones for the current phase, therapeutic exercise intensity must not exceed the limits defined for the current healing phase, and return-to-play clearance requires satisfaction of all mandatory criteria.

## Entities

The RehabilitationPlan entity carries a persistent identity throughout the recovery process, tracking status from creation through active treatment to discharge.

The ClinicalMilestone entity represents a specific recovery benchmark with a lifecycle from defined to targeted to achieved, individually trackable for progress monitoring.

The TherapeuticExerciseSet entity represents a collection of exercises prescribed for a specific phase, evolving as the clinician modifies exercises based on patient response.

The RehabilitationPhase entity represents a distinct stage of recovery with defined entry criteria, therapeutic goals, activity restrictions, and advancement criteria.

## Value Objects

HealingPhaseClassification represents the current biological healing phase (inflammatory, proliferative, remodeling) and determines appropriate therapeutic interventions.

ReturnToPlayCriterion captures a single objective benchmark for clearance, including the measurement method, threshold value, and mandatory/supplementary classification.

PainLevel represents patient-reported pain on a standardized scale with context (at rest, with activity, at worst).

WeightBearingStatus represents the permitted level of weight bearing using standard clinical classifications.

TherapeuticDosage specifies exercise parameters adjusted for rehabilitation constraints, including pain-limited repetitions, time-based holds, and protected range restrictions.

## Domain Events

RehabilitationPlanCreated is published when a new plan is established, enabling the Movement Assessment Context to schedule reassessments at appropriate recovery milestones.

MilestoneAchieved is published when a clinical milestone is met, potentially triggering phase advancement and informing stakeholders of recovery progress.

PhaseAdvanced is published when the rehabilitation plan progresses to the next phase, indicating readiness for advanced therapeutic interventions.

ReturnToPlayCleared is published when all criteria are satisfied, enabling the Exercise Prescription Context to transition the individual from rehabilitation to standard training.

## Domain Services

ReturnToPlayEvaluationService evaluates whether all return-to-play criteria have been met by aggregating current assessment data, milestone status, and clinical judgment.

PhaseAdvancementService evaluates whether phase-specific advancement criteria are sufficiently met to justify progression to the next rehabilitation phase.

TherapeuticDosageService calculates appropriate exercise dosages based on healing phase, pain response, and evidence-based rehabilitation guidelines.

## Integration Points

Rehabilitation Planning is downstream of Movement Assessment, consuming assessment data to track recovery progress. It shares a small kernel with Exercise Prescription for exercise specification concepts. It publishes ReturnToPlayCleared events consumed by Exercise Prescription to transition individuals to standard training. It consumes clinical practice guidelines from the Research and Education Context.

## Clinical Standards

Rehabilitation protocols align with evidence-based clinical practice guidelines published by the American Physical Therapy Association (APTA), the American Academy of Orthopaedic Surgeons (AAOS), and sport-specific medical organizations. Return-to-play criteria follow consensus statements from the International Olympic Committee (IOC) and sport governing bodies.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Magee, David J., and Robert C. Manske. Orthopedic Physical Assessment. 7th ed. Elsevier, 2021.
- Houglum, Peggy A., and Dolores B. Bertoti. Brunnstrom's Clinical Kinesiology. 6th ed. F.A. Davis, 2012.
- Ardern, Clare L., et al. "Consensus Statement on Return to Sport." British Journal of Sports Medicine, 2016.
