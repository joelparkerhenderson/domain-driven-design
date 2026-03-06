# Value Objects in the Nutrition Domain

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with identical attributes are considered equal and interchangeable. In the nutrition domain, value objects capture the many quantitative measurements, nutritional data points, and scientific reference values that pervade nutritional practice. Value objects enforce domain constraints through their constructors and provide type safety that prevents mixing incompatible nutritional quantities.

## Key Value Objects by Bounded Context

### Nutritional Assessment Context

**NutrientAmount**
Represents a specific quantity of a nutrient, consisting of the nutrient type (e.g., protein, iron, vitamin D), a numeric value, and a unit of measurement (grams, milligrams, micrograms, International Units). The constructor validates that the value is non-negative and the unit is appropriate for the nutrient type. Two NutrientAmount instances with the same nutrient, value, and unit are equal regardless of where they appear in the system.

**AnthropometricMeasurement**
Encapsulates a body measurement with its type (height, weight, BMI, waist circumference, skinfold thickness), numeric value, unit, and measurement method. BMI values enforce a valid physiological range. Waist circumference values are validated against biologically plausible bounds.

**Biomarker**
Represents a laboratory result with the biomarker type (serum albumin, prealbumin, hemoglobin, 25-hydroxyvitamin D), the measured value, reference range (low and high bounds), unit, and an interpretation (deficient, insufficient, adequate, elevated). The interpretation is computed from the value and reference range during construction.

**NutrientAnalysisResult**
Summarizes the comparison of an individual's nutrient intake against DRI targets. Contains a collection of NutrientAmount values representing actual intake and corresponding DRI target values, with a computed adequacy classification (inadequate, adequate, excessive) for each nutrient.

### Meal Planning Context

**MacronutrientRatio**
Represents the proportional distribution of calories from protein, carbohydrate, and fat, expressed as percentages that must sum to 100. This value object enforces the constraint that macronutrient ratios are internally consistent and fall within physiologically meaningful ranges.

**ServingSize**
Encapsulates a food portion with a numeric quantity, a unit (grams, ounces, cups, tablespoons, pieces), and an optional household description (e.g., "1 medium apple"). ServingSize provides conversion methods between units.

**NutritionalBreakdown**
Aggregates all NutrientAmount values for a recipe or meal, providing the complete macro and micronutrient profile per serving. This value object is recalculated whenever recipe ingredients change.

### Clinical Nutrition Context

**PESStatement**
Encapsulates a nutrition diagnosis in the Problem-Etiology-Signs/Symptoms format standardized by the Academy of Nutrition and Dietetics. Contains the problem label, the etiology description, and the signs/symptoms evidence. This is a value object because the diagnostic statement itself is immutable once formulated; changes create a new diagnosis entity.

**NutrientRestriction**
Represents a therapeutic limit on a specific nutrient, such as protein restricted to 0.8g/kg/day for renal patients or sodium limited to 2000mg/day for cardiac patients. Contains the nutrient type, restriction type (maximum, minimum, range), and the limit values.

**FormulaComposition**
Describes the nutritional content of an enteral or parenteral formula per unit volume, including caloric density, protein content, carbohydrate content, fat content, osmolality, and fiber content. This value object enables formula comparison and substitution decisions.

### Sports Nutrition Context

**EnergyAvailability**
Represents the calculated energy available for physiological functions after subtracting exercise energy expenditure from dietary energy intake, expressed in kcal per kg of fat-free mass per day. Values below 30 kcal/kg FFM/day flag RED-S risk. This value object enforces physiologically valid ranges and provides risk classification.

**HydrationTarget**
Specifies a fluid intake goal with fluid type (water, electrolyte solution, sports drink), volume, and timing relative to exercise (pre, during, post). The value object validates that volumes are within safe consumption rates.

### Dietary Compliance Context

**ComplianceScore**
Represents a calculated measure of dietary adherence, expressed as a percentage with component scores for macronutrient compliance, micronutrient compliance, meal timing compliance, and overall compliance. The score is computed from the comparison of logged intake against planned intake and is immutable once calculated.

**PortionEstimate**
Captures how a food portion was estimated, including the estimation method (digital scale, measuring cups, visual estimate, hand portions, photograph-based), the estimated quantity, and a confidence level. This value object captures the inherent uncertainty in dietary self-reporting.

### Outcomes Tracking Context

**BodyCompositionMeasurement**
Represents a body composition assessment at a point in time, including fat mass percentage, lean mass in kilograms, bone mineral density, and the measurement method used (DEXA, bioelectrical impedance, skinfold). Two measurements taken with different methods are not directly comparable, which is captured in the value object.

**TrendDataPoint**
Represents a single data point in a longitudinal trend, containing a timestamp, a numeric value, a unit, and the data source context. This generic value object supports building time-series views of any tracked metric.

## Design Principles

Value objects in the nutrition domain are always immutable. They validate their invariants at construction time, rejecting invalid combinations of attributes. They provide equality based on attribute comparison. They are freely shareable between entities and aggregates without risk of unintended side effects.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on value objects in business logic.
