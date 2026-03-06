# Exercise Prescription Context - Kinesiology Domain

## Overview

The Exercise Prescription Context manages the design, delivery, and progression of exercise programs. It is the primary value-generating bounded context in the kinesiology domain, translating assessment findings into structured, individualized training plans that specify exercises, dosages, progressions, and timelines. This context encapsulates the expertise of program design, including periodization theory, progressive overload principles, and evidence-based modality selection.

Exercise prescription is classified as a core subdomain because the ability to create effective, individualized programs is the defining capability of a kinesiology system. The sophistication of the prescription model directly determines the quality of clinical outcomes.

## Key Concepts

Program design encompasses the process of creating a complete training plan that addresses an individual's goals, needs, and constraints based on assessment findings. Program design decisions include exercise selection, training frequency, session structure, and the overall periodization framework.

Periodization is the systematic planning of training into distinct phases that vary in emphasis, volume, and intensity. Macrocycles span months to years and define the overall training direction. Mesocycles span weeks to months and represent focused training blocks with specific objectives. Microcycles span days to weeks and define the day-to-day training structure.

Progressive overload is the principle that training stimuli must gradually increase over time to continue driving physiological adaptation. Overload can be achieved through increases in load, volume, complexity, density, or reduced rest intervals. The rate and method of progression must be matched to the individual's training age, recovery capacity, and goals.

Modality selection determines the type of training employed, including resistance training, cardiovascular conditioning, flexibility training, plyometric training, and neuromuscular training. Selection is based on assessment findings, training goals, available equipment, and individual preferences.

Exercise selection identifies specific exercises that target identified needs while respecting contraindications and individual capacity. Selection criteria include the muscle groups targeted, the movement pattern involved, the equipment required, and the technical demands relative to the individual's skill level.

## Aggregate Design

The ExerciseProgram aggregate is the primary aggregate root. It contains the complete hierarchical specification of a training plan from macrocycle through individual exercise prescriptions. The aggregate enforces invariants including periodization phase sequencing, volume limits within mesocycles, and exercise-contraindication compatibility.

The ExerciseCatalog aggregate manages the library of available exercises with their classifications, technique specifications, regression and progression hierarchies, and contraindication profiles. This aggregate is updated infrequently and serves as a reference for program design.

## Entities

The ExerciseProgram entity represents a complete training plan with a persistent identity that spans its entire lifecycle from initial design through completion or modification.

The Mesocycle entity represents a training block within a periodized program, individually identifiable for effectiveness evaluation and progression tracking.

The TrainingSession entity represents a single planned or completed workout with a lifecycle from planned through in-progress to completed, capturing both prescribed and actual performance data.

## Value Objects

ExerciseDosage captures the prescribed parameters for an exercise: sets, repetitions, load, tempo, and rest interval as a single immutable unit.

LoadPrescription specifies training load as absolute weight, percentage of maximum, perceived exertion target, or velocity threshold.

ProgressionRule defines the criteria and method for advancing training parameters, including trigger conditions and progression magnitudes.

TrainingPhaseDescriptor characterizes a training phase by its name, duration range, primary emphasis, and target intensity zone.

## Domain Events

ProgramPrescribed is published when a new exercise program is finalized and assigned, enabling the Injury Prevention Context to begin monitoring training load.

PhaseTransitioned is published when a program moves between periodization phases, enabling cross-context monitoring of program progression.

ProgramModified is published when significant changes are made to an existing program, carrying modification details and rationale.

TrainingSessionCompleted is published when a session is completed with actual performance data, feeding workload monitoring in the Injury Prevention Context.

## Domain Services

ProgramGenerationService generates initial exercise programs from assessment findings, training goals, and available resources, applying periodization principles and exercise selection algorithms.

ProgressionCalculationService evaluates progression criteria across multiple sessions and calculates updated dosage parameters.

LoadAssignmentService translates relative load prescriptions into absolute values based on current tested or estimated maximal values.

## Integration Points

The Exercise Prescription Context is downstream of Movement Assessment through a customer-supplier relationship, consuming assessment findings to inform program design. It shares a small kernel with Rehabilitation Planning for exercise specification concepts. It is upstream to Injury Prevention, publishing training load data for workload monitoring. It consumes RiskAlertRaised events from Injury Prevention to trigger program modifications.

## Clinical Standards

Exercise prescription follows guidelines published by the American College of Sports Medicine (ACSM) for exercise testing and prescription, the National Strength and Conditioning Association (NSCA) for resistance training program design, and the American Heart Association (AHA) for cardiovascular exercise programming. Periodization models draw from the work of Tudor Bompa and contemporary periodization research.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American College of Sports Medicine. ACSM's Guidelines for Exercise Testing and Prescription. 11th ed. Wolters Kluwer, 2022.
- Bompa, Tudor, and Carlo Buzzichelli. Periodization: Theory and Methodology of Training. 6th ed. Human Kinetics, 2019.
- Haff, G. Gregory, and N. Travis Triplett. Essentials of Strength Training and Conditioning. 4th ed. Human Kinetics, 2016.
