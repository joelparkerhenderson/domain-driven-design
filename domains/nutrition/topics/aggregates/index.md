# Aggregates in the Nutrition Domain

## Overview

An aggregate is a cluster of related domain objects treated as a single unit for the purpose of data changes and consistency enforcement. Each aggregate has a root entity that serves as the sole entry point for external interactions, and the aggregate enforces invariants across all its constituent objects. In the nutrition domain, aggregates define the transactional boundaries within each bounded context, ensuring that nutritional data maintains consistency as it is created, modified, and persisted.

## Aggregates by Bounded Context

### Nutritional Assessment Context

**NutritionalAssessment (Aggregate Root)**
The NutritionalAssessment aggregate encapsulates a complete nutritional evaluation for an individual at a point in time. It contains DietaryRecall entities (each representing a 24-hour recall session with consumed food items), AnthropometricMeasurement value objects (height, weight, BMI, waist circumference, skinfold measurements), Biomarker value objects (lab results with reference ranges and interpretations), and a NutrientAnalysisResult value object summarizing macro and micronutrient intake against DRI targets. The invariant enforced is that a NutritionalAssessment cannot be finalized until at least one dietary recall and one anthropometric measurement are recorded.

**FoodItem (Aggregate Root)**
The FoodItem aggregate represents a food or beverage in the domain's food database. It contains NutrientProfile value objects detailing the macro and micronutrient content per standard serving. FoodItems are reference data that are read frequently and modified infrequently, making them a separate aggregate from NutritionalAssessment.

### Meal Planning Context

**MealPlan (Aggregate Root)**
The MealPlan aggregate organizes a collection of PlannedMeal entities over a defined time period. Each PlannedMeal references one or more Recipe aggregates and specifies portion sizes and timing. The MealPlan enforces invariants such as daily caloric and macronutrient targets falling within specified ranges and all PlannedMeals collectively meeting micronutrient minimums.

**Recipe (Aggregate Root)**
The Recipe aggregate contains Ingredient entities (each referencing a FoodItem and specifying a quantity), preparation instructions, and a calculated NutritionalBreakdown value object. Recipes are independent aggregates because they are reusable across multiple meal plans and are managed on their own lifecycle.

### Clinical Nutrition Context

**NutritionCareRecord (Aggregate Root)**
The NutritionCareRecord aggregate implements the Nutrition Care Process for a patient. It contains NutritionDiagnosis entities (each with a PES statement), NutritionIntervention entities (diet prescriptions, education plans, supplement orders), and MonitoringEvaluation value objects. The aggregate enforces clinical invariants such as every intervention requiring a linked diagnosis and parenteral nutrition orders requiring documented clinical justification.

**DietPrescription (Aggregate Root)**
The DietPrescription aggregate defines a therapeutic dietary order with NutrientRestriction value objects (e.g., protein limit of 0.8g/kg for renal patients), AllowedFoodCategory entities, and MealTimingRequirement value objects. This is a separate aggregate because prescriptions have their own approval lifecycle and may be referenced by multiple NutritionCareRecords.

### Sports Nutrition Context

**PerformanceNutritionPlan (Aggregate Root)**
The PerformanceNutritionPlan aggregate coordinates an athlete's fueling strategy across training phases. It contains FuelingPhase entities (each aligned with a training periodization phase), HydrationProtocol value objects, and SupplementationSchedule entities. The aggregate enforces that energy availability remains above safe thresholds across all phases and that supplementation does not include prohibited substances.

### Dietary Compliance Context

**ComplianceRecord (Aggregate Root)**
The ComplianceRecord aggregate tracks dietary adherence over a reporting period. It contains FoodLogEntry entities (each recording a consumed item with timestamp, quantity, and meal occasion), a ComplianceScore value object calculated by comparing logged intake against the referenced MealPlan, and BarrierAssessment value objects identifying obstacles to compliance.

### Outcomes Tracking Context

**OutcomeTimeline (Aggregate Root)**
The OutcomeTimeline aggregate assembles longitudinal health data for an individual. It contains WeightRecord value objects, BodyCompositionMeasurement value objects, BiomarkerDataPoint value objects, and PatientReportedOutcome entities. The aggregate enforces chronological ordering and validates data point ranges to catch recording errors.

## Aggregate Design Principles

Aggregates in the nutrition domain follow the principle of keeping aggregates small. Each aggregate contains only the objects necessary to enforce its invariants. Cross-aggregate references use identifiers rather than direct object references. Eventual consistency between aggregates is preferred over distributed transactions, with domain events propagating state changes across aggregate boundaries.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on aggregates.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 10 on aggregate design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on business logic with aggregates.
