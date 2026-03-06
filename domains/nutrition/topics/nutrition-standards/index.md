# Nutrition Standards in the Nutrition Domain

## Overview

Nutrition standards are scientifically established frameworks, reference values, databases, and terminologies that inform the design of value objects, validation rules, and business logic throughout the nutrition domain. These standards originate from government agencies, professional organizations, and international bodies. Incorporating them into the domain model ensures that nutritional calculations, assessments, and recommendations are grounded in evidence-based science rather than arbitrary values. In DDD terms, nutrition standards shape the published language of the domain and constrain the invariants enforced by aggregates and value objects.

## Dietary Reference Intakes (DRIs)

The Dietary Reference Intakes are a set of nutrient intake values published by the National Academies of Sciences, Engineering, and Medicine (formerly the Institute of Medicine). DRIs include the Estimated Average Requirement (EAR), the intake level meeting the needs of 50% of a population group; the Recommended Dietary Allowance (RDA), the intake level sufficient for 97-98% of a population group; the Adequate Intake (AI), used when insufficient evidence exists for an EAR; and the Tolerable Upper Intake Level (UL), the maximum intake unlikely to cause adverse effects.

In the domain model, DRI values are modeled as reference value objects parameterized by nutrient type, age group, sex, and physiological state (pregnancy, lactation). The NutrientAnalysisService uses DRI values to compute adequacy classifications. The NutrientAmount value object validates against UL values to flag potentially dangerous intake levels.

## Food Composition Databases

Food composition databases provide the nutrient content of foods, forming the foundation for all nutrient analysis calculations. The primary references include USDA FoodData Central (the United States Department of Agriculture's comprehensive food composition database), the Canadian Nutrient File, and the McCance and Widdowson's Composition of Foods (United Kingdom).

In the domain model, food composition data is accessed through the FoodItem aggregate in the Nutritional Assessment context. An anti-corruption layer translates the external database format into domain value objects. The FoodItemRepository provides search and lookup capabilities against the translated data. Food composition databases are versioned, and the domain model tracks which database version was used for each nutrient analysis to ensure reproducibility.

## International Dietetics and Nutrition Terminology (IDNT)

The IDNT, published by the Academy of Nutrition and Dietetics, standardizes the terminology used in clinical nutrition practice. It defines nutrition diagnosis labels organized into three domains (intake, clinical, and behavioral-environmental), each with specific problem categories, common etiologies, and associated signs and symptoms.

In the domain model, IDNT terminology structures the NutritionDiagnosis entity within the Clinical Nutrition context. The PES statement value object uses IDNT-standardized problem labels, and the NutritionDiagnosisService validates diagnoses against the IDNT reference set. This standardization enables clinical communication consistency and supports research data aggregation.

## Nutrition Care Process (NCP)

The Nutrition Care Process is a systematic framework consisting of four steps: nutrition assessment, nutrition diagnosis, nutrition intervention, and nutrition monitoring and evaluation. Published by the Academy of Nutrition and Dietetics, the NCP standardizes clinical nutrition practice.

In the domain model, the NCP directly shapes the NutritionCareRecord aggregate in the Clinical Nutrition context, which tracks the full process from assessment through monitoring. The aggregate's lifecycle mirrors the NCP steps, and its invariants enforce that each step follows logically from the previous one.

## Dietary Pattern Indices

Validated dietary pattern indices provide scoring systems for evaluating diet quality against evidence-based dietary patterns. Key indices include the Healthy Eating Index (HEI), scoring adherence to the Dietary Guidelines for Americans; the Mediterranean Diet Score (MDS); the DASH Diet Score; and the Alternative Healthy Eating Index (AHEI).

In the domain model, the DietaryPatternClassificationService in the Nutritional Assessment context applies these indices to dietary recall data, producing dietary quality scores as value objects.

## Sports Nutrition Standards

Sports nutrition standards include position statements from the Academy of Nutrition and Dietetics, the American College of Sports Medicine, and the International Olympic Committee. These establish evidence-based recommendations for macronutrient ranges by sport type and training phase, hydration guidelines, supplement evidence classifications (such as the Australian Institute of Sport ABCD framework), and energy availability thresholds for RED-S prevention.

In the domain model, these standards inform the invariants and validation rules in the Sports Nutrition context, particularly the EnergyAvailability value object's risk thresholds and the SupplementEvidenceService's classification criteria.

## Anthropometric Standards

Anthropometric reference standards include WHO growth charts for pediatric populations, BMI classification thresholds, and waist circumference risk cutoffs. These standards vary by population and are used to interpret anthropometric measurements within the Nutritional Assessment context.

## Impact on Domain Design

Nutrition standards are not mere reference data; they shape the structure and behavior of the domain model. DRI values define validation boundaries for value objects. IDNT terminology constrains the vocabulary of clinical entities. The NCP defines aggregate lifecycles. Dietary indices provide algorithms for domain services. Standards are versioned within the domain model because they change over time as scientific evidence evolves, and historical calculations must reference the standard version that was current when they were performed.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- National Academies of Sciences, Engineering, and Medicine. Dietary Reference Intakes. National Academies Press, various years.
- U.S. Department of Agriculture. FoodData Central. https://fdc.nal.usda.gov/
- Academy of Nutrition and Dietetics. International Dietetics and Nutrition Terminology (IDNT) Reference Manual. 2020.
