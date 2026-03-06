# Value Objects in Oncology

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. In oncology, value objects represent the many precise clinical measurements, classifications, and calculated quantities that characterize cancer care. TNM staging classifications, dose calculations, biomarker results, and performance status scores are all naturally modeled as value objects because their meaning derives from their values, not from any persistent identity.

## Characteristics of Oncology Value Objects

Value objects in oncology share several characteristics. They are immutable: once a TNM classification of T2N1M0 is created, it cannot be changed. If staging changes, a new value object is created to represent the updated classification. They are self-validating: a TNM classification validates that the T, N, and M categories are valid for the specified cancer type. They are side-effect free: operations on value objects return new value objects rather than modifying existing ones.

Immutability is particularly important in oncology because clinical values often have legal and regulatory significance. A pathology report's findings, once finalized, must not be altered. If corrections are needed, amendments are issued that reference the original values. Value objects naturally support this pattern by ensuring that historical values are preserved.

## Key Value Objects by Bounded Context

### Diagnosis and Staging Context

**TNM Classification**: Represents the tumor staging using T category (tumor extent), N category (lymph node involvement), and M category (distant metastasis). Includes the edition of the AJCC staging system used. Two TNM classifications are equal if all components match. The value object validates that the combination of categories produces a valid overall stage group for the specific cancer type.

**Histological Grade**: Represents the degree of cellular differentiation observed in the tumor specimen. Expressed as a grade value (G1 through G4 or GX) with an associated grading system (Gleason for prostate, Nottingham for breast, FIGO for gynecological cancers). The value object encapsulates both the grade and the grading system that produced it.

**Biomarker Result**: Represents the result of a specific biomarker test, including the biomarker name, the measured value, the interpretation (positive, negative, equivocal), and the testing methodology used. A HER2 result of 3+ by immunohistochemistry is a distinct value object from a HER2 result of amplified by FISH, even though both indicate HER2 positivity.

**Molecular Mutation**: Represents a specific genetic alteration identified in the tumor, including the gene name, the mutation type (point mutation, deletion, amplification, fusion), and the variant classification (pathogenic, likely pathogenic, variant of uncertain significance).

### Chemotherapy Management Context

**Dose Calculation**: Represents the calculated dose for a specific drug for a specific patient, including the drug identifier, the protocol-defined dose rate (mg/m2 or mg/kg), the patient's body surface area or weight, the calculated absolute dose, any dose reduction percentage with clinical justification, and the final administered dose.

**Body Surface Area**: Represents a patient's calculated body surface area in square meters, derived from height and weight using a specified formula (Mosteller, DuBois, or Haycock). This value object is used as input to dose calculations and is recalculated when patient weight changes.

**CTCAE Grade**: Represents the severity of an adverse event using the Common Terminology Criteria for Adverse Events grading scale. Includes the CTCAE version, the adverse event term, the system organ class, and the grade (1 through 5). Two CTCAE grades are equal if all components match.

**Regimen Protocol**: Represents the standardized definition of a chemotherapy regimen, including the drug combination, standard dose rates, administration routes, cycle length, and number of planned cycles. The protocol is a value object because it describes the canonical regimen definition, not a patient-specific order.

### Radiation Therapy Context

**Radiation Dose**: Represents a radiation dose quantity in Gray (Gy) or centigray (cGy), with a specified dose type (prescribed, planned, delivered). Two radiation dose objects are equal if they represent the same quantity in the same unit with the same type designation.

**Dose-Volume Constraint**: Represents a constraint on radiation dose to a specific anatomical structure, expressed as a volume percentage receiving no more than a specified dose (for example, V20 less than 30% for lung). Used in treatment plan optimization and evaluation.

**Fractionation Scheme**: Represents the division of total radiation dose into individual fractions, including the total dose, dose per fraction, number of fractions, and treatment frequency (daily, twice daily, alternate days). Common schemes such as conventional fractionation (2 Gy per fraction) and hypofractionation (greater than 2 Gy per fraction) are captured as value objects.

### Surgical Oncology Context

**Surgical Margin**: Represents the pathological assessment of the tissue border after resection. Includes the margin status (positive, negative, close), the closest margin distance in millimeters, and the location. Two margin assessments are equal if all components match.

**Lymph Node Status**: Represents the aggregate assessment of lymph node involvement, including the number of nodes examined, the number of nodes positive for cancer, and the presence of extranodal extension.

### Survivorship Care Context

**Performance Status**: Represents a patient's functional capacity using a standardized scale. Includes the scale used (ECOG or Karnofsky) and the score value. The value object validates that the score falls within the valid range for the specified scale.

**Follow-Up Interval**: Represents a recommended surveillance interval, including the starting point (months from treatment completion), the test or examination to be performed, and the frequency. Two follow-up intervals are equal if all components match.

## Value Object Composition

Oncology value objects are often composed of other value objects. A Cancer Diagnosis Summary value object might contain a TNM Classification, a Histological Grade, and a set of Biomarker Results. This composition creates rich, self-describing domain concepts that carry all the information needed for clinical decision-making without requiring separate lookups.

## Value Object Operations

Value objects can expose operations that return new value objects. A Dose Calculation value object can expose a reduce operation that takes a reduction percentage and returns a new Dose Calculation with the adjusted dose. A TNM Classification can expose a stageGroup operation that returns the corresponding overall stage group as a new value object.

## Equality and Comparison

Value object equality in oncology must account for clinical precision. Two radiation doses of 60.0 Gy and 60.00 Gy should be considered equal. Two body surface area values calculated from the same height and weight but using different formulas should be considered unequal because the calculation methodology is part of the value's meaning.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5: A Model Expressed in Software.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6: Value Objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8: Domain Model Patterns.
