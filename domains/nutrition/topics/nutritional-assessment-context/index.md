# Nutritional Assessment Context

## Overview

The Nutritional Assessment Context is responsible for the systematic evaluation of an individual's nutritional status. It captures dietary intake data through validated recall methods, records anthropometric body measurements, integrates laboratory-based biomarkers, and performs nutrient analysis against established reference values. This context forms the foundation of the nutrition domain: every downstream intervention, whether clinical, athletic, or behavioral, depends on accurate assessment data.

This bounded context is classified as a core subdomain because the quality of nutritional assessment directly determines the quality and safety of all subsequent nutritional decisions. The domain logic for interpreting dietary recall data, matching foods to composition databases, and synthesizing multiple data sources into a nutritional status determination requires deep domain expertise.

## Ubiquitous Language

Within this context, "assessment" means a comprehensive evaluation of nutritional status, not a general evaluation. "Intake" refers to reported food consumption data captured through recall methods. "Analysis" means the computational process of deriving nutrient values from food intake records. "Adequacy" is the comparison of actual intake against DRI reference values. "Status" is the synthesized conclusion about an individual's nutritional condition.

## Aggregates

**NutritionalAssessment** is the primary aggregate root. It contains DietaryRecall entities, AnthropometricMeasurement value objects, Biomarker value objects, and a NutrientAnalysisResult value object. The invariant is that an assessment cannot be finalized without at least one dietary recall and one anthropometric measurement.

**FoodItem** is a separate aggregate root representing reference data from the food composition database. It contains NutrientProfile value objects detailing macro and micronutrient content per standard serving.

## Entities

**DietaryRecall** represents a single recall session covering a defined time period (typically 24 hours). It has a unique identifier, a date, a method type (24-hour recall, food frequency questionnaire, food diary), and a collection of consumed items with quantities and preparation methods.

**AssessmentSubject** represents the individual being assessed, carrying demographic attributes relevant to nutritional evaluation (age, sex, physiological state, activity level).

## Value Objects

**NutrientAmount** captures a quantity of a specific nutrient with its unit. **AnthropometricMeasurement** encapsulates a body measurement with its type, value, unit, and method. **Biomarker** represents a lab result with its type, value, reference range, and interpretation. **NutrientAnalysisResult** summarizes intake-to-DRI comparisons across all nutrients.

## Domain Events

**AssessmentCompleted** is raised when an assessment is finalized, carrying key findings for consumption by Clinical Nutrition, Sports Nutrition, and Outcomes Tracking. **NutrientDeficiencyIdentified** is raised when a specific nutrient falls below reference thresholds. **BiomarkerResultRecorded** is raised when a new lab result is added.

## Domain Services

**NutrientAnalysisService** computes nutrient profiles from dietary recalls by matching foods against the composition database. **DietaryPatternClassificationService** classifies eating patterns against published dietary quality indices. **NutritionalStatusScoringService** synthesizes data into composite screening scores using validated tools like MUST or MNA.

## Repositories

**NutritionalAssessmentRepository** provides access to assessment aggregates with operations for finding by subject, date range, and deficiency criteria. **FoodItemRepository** provides access to the food composition reference data with search by name, category, and nutrient content.

## Integration Points

This context is upstream to Clinical Nutrition and Sports Nutrition, supplying assessment data through domain events. It integrates with external food composition databases (USDA FoodData Central) through an anti-corruption layer and with laboratory information systems for biomarker data through another ACL. The food composition database integration uses an Open Host Service pattern.

## Clinical and Regulatory Considerations

Assessment methods must follow validated protocols. The 24-hour dietary recall should follow the USDA Automated Multiple-Pass Method. Anthropometric measurements must use standardized techniques. Biomarker interpretation must reference current clinical guidelines. All assessment data is considered protected health information and subject to privacy regulations.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Mahan, L. Kathleen, and Janice L. Raymond. Krause and Mahan's Food and the Nutrition Care Process. Elsevier, 2020.
- Thompson, Frances E., and Amy F. Subar. "Dietary Assessment Methodology." Nutrition in the Prevention and Treatment of Disease, 2017.
