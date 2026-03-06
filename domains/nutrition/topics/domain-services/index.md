# Domain Services in the Nutrition Domain

## Overview

A domain service is a stateless operation that represents significant domain behavior which does not naturally belong to any single entity or value object. Domain services are used when an operation involves multiple aggregates, crosses bounded context concepts, or encapsulates a complex domain algorithm. In the nutrition domain, domain services implement calculations, analyses, and coordination logic that span the rich data structures within each bounded context.

## Domain Services by Bounded Context

### Nutritional Assessment Context

**NutrientAnalysisService**
This service computes the complete nutrient profile of a dietary recall by matching consumed food items against the food composition database, summing nutrient quantities across all items, adjusting for cooking methods and bioavailability factors, and comparing totals against applicable DRI values. This operation spans the FoodItem and NutritionalAssessment aggregates, making it inappropriate for either aggregate to own. The service takes a DietaryRecall entity and a collection of FoodItem references as input and produces a NutrientAnalysisResult value object.

**DietaryPatternClassificationService**
This service analyzes a dietary recall or food frequency questionnaire to classify the individual's eating pattern against established dietary pattern indices (Healthy Eating Index, Mediterranean Diet Score, DASH score). The classification algorithm applies scoring criteria from published dietary pattern indices, requiring knowledge that spans food items, nutrient amounts, and food group categorizations.

**NutritionalStatusScoringService**
This service synthesizes anthropometric, biochemical, and dietary data into a composite nutritional status score. It applies validated screening tools such as the Malnutrition Universal Screening Tool (MUST) or the Mini Nutritional Assessment (MNA), combining data from multiple value objects within the NutritionalAssessment aggregate.

### Meal Planning Context

**MealPlanOptimizationService**
This service generates or adjusts meal plans to meet specified nutritional targets while respecting dietary preferences, food allergies, budget constraints, and recipe availability. The optimization algorithm spans the MealPlan and Recipe aggregates, selecting and combining recipes to achieve target macronutrient ratios and micronutrient minimums. This is inherently a cross-aggregate operation.

**GroceryListAggregationService**
This service generates a consolidated grocery list from a meal plan by extracting ingredients from all referenced recipes, aggregating quantities of identical items, converting between units as needed, and organizing items by food category. This operation reads from multiple Recipe aggregates and produces a GroceryList value object.

### Clinical Nutrition Context

**NutritionDiagnosisService**
This service assists clinicians in formulating nutrition diagnoses by analyzing assessment data against clinical criteria and suggesting appropriate IDNT problem labels, etiologies, and signs/symptoms. The service encapsulates the clinical decision logic for identifying nutrition problems from assessment data, operating across the NutritionCareRecord and the assessment data received from the Nutritional Assessment context.

**EnteralFormulaSelectionService**
This service recommends appropriate enteral feeding formulas based on a patient's clinical condition, nutrient requirements, and tolerance history. It evaluates formula options against the patient's NutrientRestriction values and clinical requirements, comparing FormulaComposition value objects from the available formulary.

**NutrientDrugInteractionService**
This service checks for interactions between a patient's prescribed medications and their dietary intake or supplement regimen. It cross-references dietary data with known nutrient-drug interactions, producing interaction warnings that inform diet prescription modifications.

### Sports Nutrition Context

**EnergyAvailabilityCalculationService**
This service calculates energy availability by subtracting exercise energy expenditure from dietary energy intake and normalizing per kilogram of fat-free mass. The calculation spans dietary intake data (from assessment or compliance), exercise data, and body composition data (from outcomes tracking), making it unsuitable for any single aggregate. The service produces an EnergyAvailability value object and evaluates RED-S risk.

**PeriodizedFuelingService**
This service generates phase-specific macronutrient targets based on an athlete's training plan, body composition, sport demands, and performance goals. It calculates carbohydrate periodization recommendations (e.g., g/kg body weight targets for training days vs. rest days), protein timing around training sessions, and fat targets for hormonal health.

### Dietary Compliance Context

**AdherenceScoringService**
This service calculates compliance scores by comparing food log entries against the active meal plan. It matches logged foods to planned foods, evaluates portion accuracy, assesses meal timing adherence, and computes component and overall compliance scores. The service operates across the ComplianceRecord aggregate and the meal plan reference data.

**BarrierAnalysisService**
This service analyzes patterns in compliance data to identify systematic barriers to dietary adherence. It looks for temporal patterns (weekend non-compliance, evening overeating), situational patterns (travel, social events), and nutritional patterns (specific nutrient targets consistently missed) using statistical analysis of compliance records over time.

### Outcomes Tracking Context

**TrendAnalysisService**
This service performs longitudinal trend analysis on outcome data points, calculating moving averages, rates of change, and statistical significance of observed trends. It operates across multiple data point types within the OutcomeTimeline aggregate and generates trend assessments and alerts when patterns indicate clinical concern.

## Design Principles

Domain services in the nutrition domain are stateless, receiving all required data through method parameters and returning results without modifying any persistent state directly. They are defined as interfaces in the domain layer and may have infrastructure-aware implementations when they require access to external calculation libraries or reference databases.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on domain services.
