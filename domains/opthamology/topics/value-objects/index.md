# Value Objects in Ophthalmology

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather
than by a unique identity. Two value objects with the same attributes are considered
equal. In the ophthalmology domain, value objects represent clinical measurements,
classifications, and descriptive data that are meaningful as complete units but do not
need to be tracked individually over time. Value objects are fundamental building blocks
of the ophthalmology domain model, capturing the precision required by clinical practice.

## Value Object Characteristics

Value objects in the ophthalmology domain exhibit these properties:

- Immutability: once created, a value object cannot be modified. A new value object
  is created to represent a changed value.
- Equality by attributes: two VisualAcuity value objects are equal if they have the
  same notation system, numerator, and denominator.
- Self-validation: value objects validate their own invariants upon creation. An
  IntraocularPressure value object rejects negative values or values exceeding
  physiological limits.
- Replaceability: value objects are replaced wholesale rather than modified in place.

## Value Objects by Bounded Context

### Clinical Examination Context

**VisualAcuity**: Represents a visual acuity measurement. Attributes include notation
system (Snellen, LogMAR, Jaeger), distance or near designation, measured value, and
correction status (uncorrected, best-corrected, pinhole). Self-validates that the
value is within recognized ranges.

**RefractionPrescription**: Represents a complete refraction result. Attributes include
sphere power (diopters), cylinder power (diopters), axis (degrees 1-180), add power
for near correction, and vertex distance. Self-validates axis range and sign conventions.

**IntraocularPressure**: Represents a single IOP measurement. Attributes include
pressure value (mmHg), measurement method, and central corneal thickness correction
factor. Self-validates that pressure is within physiological bounds (0-80 mmHg).

**EyeLaterality**: Represents which eye is being referenced. Values: OD (right eye),
OS (left eye), OU (both eyes). Used universally across all bounded contexts as a
shared value object.

**AnteriorSegmentFinding**: Represents a structured finding from slit lamp examination.
Attributes include anatomical location, finding type, severity grade, and descriptive
text.

### Diagnostic Imaging Context

**ScanProtocol**: Represents the technical parameters of an imaging scan. Attributes
include scan pattern (raster, radial, cube), scan dimensions, resolution, and number
of averaged frames.

**RetinalThickness**: Represents a retinal thickness measurement from OCT. Attributes
include thickness value (micrometers), measurement location (e.g., ETDRS grid sector),
and normative comparison status.

**MeanDeviation**: Represents the mean deviation statistic from visual field testing.
Attributes include value (decibels), probability level, and comparison to age-matched
norms.

**PatternStandardDeviation**: Represents the pattern standard deviation from visual
field testing. Attributes include value (decibels) and probability level.

### Surgical Management Context

**IOLSpecification**: Represents the specifications of an intraocular lens implant.
Attributes include manufacturer, model, power (diopters), optic material, haptic
design, and target refraction.

**SurgicalRiskScore**: Represents a calculated risk assessment for a surgical
procedure. Attributes include risk level, contributing factors, and calculation
methodology.

**WoundArchitecture**: Represents the surgical wound characteristics. Attributes
include incision type, location (clock hour), width, and construction technique.

### Refractive Services Context

**CornealTopography**: Represents corneal surface curvature data. Attributes include
keratometry values (steep K, flat K), axis of astigmatism, corneal irregularity
index, and topographic pattern classification.

**Pachymetry**: Represents corneal thickness measurement. Attributes include central
corneal thickness (micrometers), measurement method (ultrasonic, optical), and
thinnest point location.

**WavefrontAberrometry**: Represents higher-order optical aberrations. Attributes
include Zernike coefficients, root mean square values, and total higher-order
aberration measurement.

### Glaucoma Management Context

**TargetIOP**: Represents the clinically determined target intraocular pressure for
a glaucoma patient. Attributes include target value (mmHg), rationale, and setting
date.

**CupToDiscRatio**: Represents the ratio of optic cup to optic disc diameter.
Attributes include horizontal and vertical ratios, assessment method, and laterality.

**VisualFieldIndex**: Represents a summary index from visual field testing. Attributes
include VFI percentage, reliability indices, and trend significance.

### Retinal Care Context

**DRSeverityGrade**: Represents the severity classification of diabetic retinopathy.
Attributes include severity level (none, mild NPDR, moderate NPDR, severe NPDR, PDR),
classification system (ETDRS, International Clinical), and qualifying findings.

**AMDClassification**: Represents the classification of age-related macular
degeneration. Attributes include type (dry/wet), stage, presence of drusen
characteristics, and geographic atrophy or choroidal neovascularization status.

**InjectionDose**: Represents the medication dosage for an intravitreal injection.
Attributes include drug name, concentration, volume, and preparation instructions.

## Value Object Design Guidelines

- Prefer value objects over primitive types. Use VisualAcuity instead of a raw string
  or number to capture the full semantics of a measurement.
- Make value objects self-validating. Invalid clinical measurements should be rejected
  at construction time.
- Value objects may contain behavior. A RefractionPrescription can calculate spherical
  equivalent. An IntraocularPressure can apply a corneal thickness correction.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5.
