# Value Objects - Kinesiology Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal regardless of how or when they were created. Value objects are ideal for representing measurements, quantities, descriptors, and other concepts where the values themselves matter more than tracking a specific instance over time.

In the kinesiology domain, value objects are pervasive. Measurements like joint angles, force magnitudes, training loads, and pain levels are naturally modeled as value objects. They carry precise quantitative or qualitative information, enforce validity constraints through construction, and ensure consistency through immutability.

## Movement Assessment Context Value Objects

### JointAngle

Represents a measured angle at a specific joint in a specific plane of motion. Attributes include the joint (shoulder, knee, hip), the plane (sagittal, frontal, transverse), the measurement type (active, passive, functional), and the value in degrees. Construction validates that the angle falls within anatomically plausible ranges for the specified joint and plane.

### MuscleGrade

Represents a manual muscle test result using the standard Medical Research Council scale (0 through 5). The value object enforces that grades use recognized values and supports the plus/minus modifiers (4+, 4-, 3+) used in clinical practice. Two MuscleGrade objects with the same grade value for the same muscle group are equal.

### MovementScore

Represents a scored outcome from a standardized movement screen. Attributes include the test name, the score value, and the scoring criteria version. The value object ensures scores fall within the valid range for the specified test (for example, 0 to 3 for each Functional Movement Screen test).

### GaitParameter

Represents a derived gait measurement such as stride length, cadence, stance phase percentage, or gait speed. Attributes include the parameter type, the measured value, and the unit of measurement. Construction validates that values are physiologically plausible.

### BodySegmentLength

Represents the measured length of a body segment used in biomechanical calculations. Attributes include the segment identifier, the length in centimeters, and the measurement method. Immutability ensures that biomechanical calculations remain consistent with the measurements they were based on.

## Exercise Prescription Context Value Objects

### ExerciseDosage

Represents the prescribed parameters for an exercise: sets, repetitions, load, tempo, and rest interval. This value object encapsulates the complete dosage prescription as a single immutable unit. Two ExerciseDosage objects with identical parameters are considered equal, reflecting the fact that the prescription is the same regardless of which program contains it.

### LoadPrescription

Represents a training load specification, which may be expressed as absolute weight, percentage of one-repetition maximum, rating of perceived exertion target, or velocity-based threshold. Attributes include the load type, the value, and the reference baseline if applicable.

### ProgressionRule

Represents the criteria and method for advancing training parameters. Attributes include the trigger condition (such as completing all prescribed repetitions for two consecutive sessions), the progression magnitude (such as increase load by 2.5 kilograms), and the parameter being progressed.

### TrainingPhaseDescriptor

Represents the characteristics of a training phase within a periodized program. Attributes include the phase name (anatomical adaptation, hypertrophy, strength, power, peaking), the typical duration range, the primary training emphasis, and the target intensity zone.

## Rehabilitation Planning Context Value Objects

### HealingPhaseClassification

Represents the current biological healing phase of injured tissue. Values include inflammatory (days 0-7), proliferative (days 7-21), and remodeling (days 21 onward). The classification determines which therapeutic interventions are appropriate and which are contraindicated.

### ReturnToPlayCriterion

Represents a single objective benchmark for return-to-play clearance. Attributes include the criterion description, the measurement method, the threshold value, and whether it is mandatory or supplementary. Examples include achieving 90 percent or greater limb symmetry index on hop testing or demonstrating full pain-free range of motion.

### PainLevel

Represents a patient-reported pain rating on a standardized scale. Attributes include the scale type (visual analog scale, numeric rating scale), the value, and the context (at rest, with activity, at worst). The value object validates that the reported value falls within the scale's valid range.

### WeightBearingStatus

Represents the permitted level of weight bearing during rehabilitation. Values follow the standard clinical classification: non-weight-bearing, toe-touch weight-bearing, partial weight-bearing, weight-bearing as tolerated, and full weight-bearing.

## Performance Analysis Context Value Objects

### GroundReactionForce

Represents a three-dimensional force vector measured by a force plate. Attributes include the vertical, anteroposterior, and mediolateral force components, all expressed in newtons. The value object supports vector operations including magnitude calculation and component ratio analysis.

### RateOfForceDevelopment

Represents the slope of the force-time curve over a specified interval. Attributes include the rate value (newtons per second), the measurement interval, and the analysis method. This metric is critical for assessing explosive strength capacity.

### PowerOutput

Represents mechanical power calculated from force and velocity data. Attributes include the power value in watts, the calculation method (peak, mean, or time-specific), and the movement task context.

## Injury Prevention Context Value Objects

### AcuteChronicWorkloadRatio

Represents the ratio of recent training load to longer-term average load. Attributes include the acute window (typically 7 days), the chronic window (typically 28 days), the calculation method (rolling average or exponentially weighted), and the resulting ratio value.

### RiskScore

Represents a calculated injury risk level based on aggregated risk factors. Attributes include the score value, the risk classification (low, moderate, high, very high), and the contributing factors. The value object ensures that classification thresholds are consistently applied.

## Value Object Design Principles

Value objects must be immutable. Once created, their attributes never change. To represent a different value, create a new value object rather than modifying an existing one.

Value object equality is determined by comparing all attributes. Two JointAngle objects are equal if and only if they represent the same joint, plane, measurement type, and degree value.

Value objects should validate their invariants at construction time. A JointAngle that represents a knee flexion angle of 300 degrees should be rejected during construction because it is anatomically impossible.

Value objects should be side-effect-free. Methods on value objects should return new value objects or primitive results without modifying any state.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value object design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on value object patterns.
