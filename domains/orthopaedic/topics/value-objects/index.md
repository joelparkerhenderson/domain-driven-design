# Value Objects in the Orthopaedic Domain

## Overview

A value object is an immutable domain object defined entirely by its attributes, with
no conceptual identity. Two value objects with the same attributes are considered equal.
In the orthopaedic domain, value objects capture clinical measurements, classifications,
scores, and anatomical descriptors that are defined by their content rather than by
a unique identifier.

## Purpose

Orthopaedic medicine relies on precise measurements, standardized classifications,
and validated scoring systems. These concepts are naturally modeled as value objects
because their meaning comes from their values, not from tracking a specific instance
over time. A range-of-motion measurement of 90 degrees flexion is equal to any other
measurement of 90 degrees flexion regardless of when or where it was recorded.

## Key Value Objects

### Clinical Measurements

**RangeOfMotion**: Captures joint movement in degrees with properties for flexion,
extension, abduction, adduction, internal rotation, and external rotation. Includes
the joint measured and whether the measurement is active or passive.

**GoniometryReading**: Records a specific angular measurement with the joint, plane
of motion, measured angle in degrees, and date of measurement.

**MuscleStrength**: Records manual muscle testing using the Medical Research Council
(MRC) scale (0-5), specifying the muscle group and laterality.

**LimbLengthMeasurement**: Records the measured length of a limb segment in
millimeters, with the measurement method (clinical or radiographic) and anatomical
landmarks used.

### Classification Values

**AOOTACode**: Represents a fracture classification using the AO/OTA alphanumeric
system. Contains bone identifier (e.g., 31 for proximal tibia), fracture type (A, B, C),
group, and subgroup. Immutable once assigned.

**FracturePattern**: Describes the morphology of a fracture: simple, wedge, or
comminuted, with additional descriptors for displacement and angulation.

**JointGrade**: Represents the Kellgren-Lawrence grade (0-4) for osteoarthritis
severity, or other grading systems used in orthopaedic assessment.

### Implant Specifications

**ImplantSize**: Captures the dimensional specification of an implant component,
including size designation, material, and manufacturer catalog number.

**BearingSurfacePairing**: Describes the articulating material combination (e.g.,
ceramic-on-polyethylene, metal-on-metal) as an immutable pair.

**CementType**: Specifies the bone cement used, including brand, antibiotic loading,
and viscosity classification.

### Outcome Scores

**OxfordHipScore**: A validated 12-item patient-reported outcome measure with a
total score from 0 (worst) to 48 (best), recorded as an immutable snapshot.

**OxfordKneeScore**: The knee-specific equivalent, also 0-48, capturing
patient-reported function and pain.

**WOMACScore**: The Western Ontario and McMaster Universities Osteoarthritis Index,
with subscales for pain, stiffness, and physical function.

**VASPainScore**: A visual analog scale pain measurement from 0 (no pain) to 100
(worst pain imaginable).

### Anatomical Descriptors

**Laterality**: Left, right, or bilateral, ensuring anatomical side is never ambiguous.

**AnatomicalSite**: A structured descriptor combining bone, segment, and side to
precisely identify the location of a condition or procedure.

**SurgicalApproachType**: The named surgical approach (e.g., posterior, anterior,
lateral, medial parapatellar) as an enumerated value.

## Value Object Design Principles

- Immutability: Once created, a value object's attributes never change. Create a new
  instance to represent a different value.
- Equality by attributes: Two value objects are equal when all their attributes are equal.
- Self-validation: Value objects validate their attributes at construction time.
  An AOOTACode rejects an invalid bone-type combination.
- Side-effect-free behavior: Methods on value objects return new value objects or
  primitive results without modifying state.
- Replaceability: A value object can be replaced entirely rather than modified.

## Clinical Safety Considerations

Value objects enforce clinical constraints at the type level. A RangeOfMotion object
rejects negative angles. An AOOTACode rejects codes that do not exist in the
classification system. This makes invalid clinical data unrepresentable.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 10.
