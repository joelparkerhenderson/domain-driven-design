# Entities in the Nutrition Domain

## Overview

An entity is a domain object defined by its unique identity rather than its attributes. Entities have a lifecycle: they are created, modified over time, and eventually archived or deleted, but their identity persists throughout. In the nutrition domain, entities represent things that must be tracked individually over time, where distinguishing one instance from another matters for clinical accuracy, compliance tracking, and outcome measurement.

## Key Entities by Bounded Context

### Nutritional Assessment Context

**DietaryRecall**
A DietaryRecall entity represents a single dietary recall session, typically covering a 24-hour period. It has a unique identifier, a date of recall, a recall method (24-hour recall, food frequency questionnaire, or food diary), and a collection of consumed food items with quantities. Two dietary recalls conducted on different dates for the same individual are distinct entities even if the individual consumed identical foods, because the temporal context matters for nutritional assessment.

**AssessmentSubject**
The AssessmentSubject entity represents the individual being assessed. It carries a unique identifier and demographic attributes relevant to nutritional assessment (age, sex, physiological state such as pregnancy or lactation). The AssessmentSubject persists across multiple assessments, linking longitudinal data.

### Meal Planning Context

**PlannedMeal**
A PlannedMeal entity represents a specific meal within a meal plan, identified uniquely by its position in the plan (day and meal occasion). It references one or more recipes, specifies portion adjustments, and carries scheduling information. Each PlannedMeal is distinct because substitutions, timing changes, and portion modifications must be tracked individually.

**Ingredient**
An Ingredient entity within a Recipe references a FoodItem and specifies a quantity and unit. While the FoodItem itself is a reference aggregate, the Ingredient entity is unique within its recipe because the same FoodItem may appear multiple times with different quantities or preparation methods.

### Clinical Nutrition Context

**NutritionDiagnosis**
A NutritionDiagnosis entity captures a formally identified nutrition problem using the IDNT PES (Problem-Etiology-Signs/Symptoms) format. Each diagnosis has a unique identifier, a status (active, resolved, inactive), and links to the assessment data that supports it. The diagnosis identity persists as its status changes through the care process.

**NutritionIntervention**
A NutritionIntervention entity represents a specific action taken to address a nutrition diagnosis. This may be a dietary modification, nutrition education session, coordination of care, or a nutrition supplement order. Each intervention has a unique identifier, tracks its implementation status, and links to one or more nutrition diagnoses.

**EnteralFeedingOrder**
An EnteralFeedingOrder entity represents a prescribed tube feeding regimen. It specifies the formula product, delivery rate, volume, and schedule. The order has a lifecycle from prescription through administration and monitoring, and must be tracked individually for clinical safety and regulatory compliance.

### Sports Nutrition Context

**FuelingPhase**
A FuelingPhase entity aligns a specific nutrition strategy with a training periodization phase (base, build, peak, recovery, or competition). Each phase has unique macronutrient targets, meal timing recommendations, and pre/during/post-exercise fueling protocols. Phases are tracked individually because nutritional needs change dramatically between training phases.

**SupplementationSchedule**
A SupplementationSchedule entity defines a specific supplement within an athlete's regimen, including the supplement identity, dosage, timing, duration, and the evidence basis for its inclusion. Each schedule entry is individually tracked for compliance monitoring and adverse effect reporting.

### Dietary Compliance Context

**FoodLogEntry**
A FoodLogEntry entity records a single food or beverage consumption event. It has a unique identifier, a timestamp, a food item reference, a quantity with portion estimation method, and a meal occasion classification (breakfast, lunch, dinner, snack). Individual log entries must be tracked separately because they form the basis of adherence calculations.

**CoachingIntervention**
A CoachingIntervention entity represents a behavioral coaching action delivered to address a compliance barrier. Each intervention has a type (motivational interviewing, goal setting, stimulus control), a delivery date, and an outcome assessment. Interventions are tracked individually to evaluate which coaching strategies are effective.

### Outcomes Tracking Context

**PatientReportedOutcome**
A PatientReportedOutcome entity captures a subjective health measure reported by the individual at a specific time. Examples include perceived energy level, appetite rating, gastrointestinal comfort, and overall quality of life. Each report is individually identified because tracking changes in subjective measures over time requires distinct instances.

## Entity Design Principles

Entities in the nutrition domain follow the principle that identity is the defining characteristic. Attribute equality does not imply entity equality. Two FoodLogEntries with identical food items and quantities at different timestamps are different entities. Entity identity is typically implemented through domain-generated identifiers rather than database-assigned keys, following DDD best practices.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on entities.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 5 on entity design.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on implementing business logic.
