# Nutrition Management Context

## Overview

The Nutrition Management Context handles nutritional assessment, intervention planning,
and dietary therapy management for gastroenterology patients. This context covers
enteral and parenteral nutrition, celiac disease management, food intolerance evaluation,
malnutrition screening, and dietary intervention tracking. Nutritional management is
essential in gastroenterology because many GI disorders directly impair nutrient
absorption, intake, or metabolism.

## Scope and Responsibilities

This context is responsible for:

- Conducting comprehensive nutritional assessments using validated screening tools.
- Classifying malnutrition risk and severity.
- Planning enteral nutrition including oral supplements and tube feeding formulations.
- Planning parenteral nutrition with macro and micronutrient specifications.
- Managing celiac disease through diagnosis confirmation, gluten-free diet counseling,
  serologic monitoring, and adherence assessment.
- Evaluating food intolerances including lactose intolerance, fructose malabsorption,
  and other carbohydrate intolerances.
- Designing and monitoring elimination diets and reintroduction protocols.
- Tracking dietary intervention adherence and clinical outcomes.
- Calculating individualized nutrient requirements based on clinical condition.

## Core Aggregates

### NutritionalAssessment

Represents a comprehensive evaluation of a patient's nutritional status at a specific
point in time. Contains anthropometric measurements (height, weight, BMI, body
composition), biochemical markers (albumin, prealbumin, transferrin, micronutrient
levels), dietary intake analysis (caloric intake, macronutrient distribution), clinical
signs of deficiency, and malnutrition risk classification.

Key invariants: risk classification must follow a validated screening tool algorithm
(e.g., Malnutrition Universal Screening Tool, Subjective Global Assessment). All
anthropometric measurements must be physiologically plausible. BMI must be correctly
calculated from height and weight.

### NutritionPlan

Represents a specific nutritional intervention prescription for a patient. Contains
the delivery method (oral, enteral tube, parenteral), formulation specifications,
caloric targets, protein targets, micronutrient supplementation, fluid requirements,
feeding schedule, and monitoring parameters.

Key invariant: caloric and protein targets must be justified by the patient's clinical
condition, body weight, and metabolic stress level. Parenteral nutrition formulations
must specify all required components (amino acids, dextrose, lipids, electrolytes,
vitamins, trace elements). Enteral formulations must specify product, rate, and
delivery method.

### DietaryIntervention

Represents a specific dietary therapy such as a gluten-free diet for celiac disease,
a low-FODMAP diet for IBS, or an elimination diet for food intolerance evaluation.
Contains the intervention type, dietary restrictions, permitted alternatives, duration,
adherence assessment methodology, and outcome tracking.

Key invariant: elimination diets must have a defined duration and a structured
reintroduction protocol. Celiac disease dietary management must include serologic
monitoring intervals.

## Key Entities

- MealPlan: a structured daily dietary plan with meals, portions, and nutritional
  content calculations.
- SupplementRegimen: prescribed nutritional supplements with dosing, timing, and
  monitoring requirements.
- AdherenceRecord: documentation of patient adherence to the dietary intervention
  with barriers identified and counseling provided.

## Key Value Objects

- BMI: body mass index calculated from height and weight with WHO classification.
- CaloricTarget: daily caloric goal in kcal with calculation methodology documented.
- ProteinTarget: daily protein goal in grams with per-kg-body-weight basis.
- MicronutrientLevel: specific vitamin or mineral measurement with reference range
  and deficiency classification.
- MalnutritionRiskScore: validated screening tool score with risk category.
- DietaryRestriction: specific food or food group restriction with clinical rationale.
- TissueTransglutaminaseLevel: celiac disease serologic marker with interpretation.

## Domain Events

- NutritionalRiskIdentified: raised when screening identifies a patient at nutritional
  risk, triggering comprehensive assessment.
- NutritionPlanModified: raised when an active nutrition plan is adjusted, triggering
  supply coordination and counseling.
- NutritionalGoalAchieved: raised when target nutritional parameters are met.
- CeliacSerologyChanged: raised when celiac disease monitoring shows significant
  serologic changes, indicating adherence issues or disease activity.
- RefeedingSyndromeRiskDetected: raised when a severely malnourished patient is at
  risk for refeeding syndrome, triggering cautious nutrition initiation protocols.

## Clinical Workflows

### Malnutrition Screening Workflow

All gastroenterology patients undergo nutritional screening using a validated tool.
Patients identified as at risk receive comprehensive nutritional assessment. The
NutritionalAssessment aggregate is created with full evaluation data, and the
NutritionalRiskIdentified event is raised.

### Enteral Nutrition Workflow

Patients unable to meet nutritional needs through oral intake receive enteral nutrition
planning. The NutritionPlan aggregate specifies the formulation, delivery method
(nasogastric, nasojejunal, PEG, PEJ), rate, and schedule. Monitoring includes
tolerance assessment, residual checks, and periodic nutritional reassessment.

### Celiac Disease Management Workflow

Patients diagnosed with celiac disease receive dietary counseling for strict gluten-free
diet adherence. The DietaryIntervention aggregate tracks the gluten-free diet
prescription, adherence assessments, tissue transglutaminase monitoring, and clinical
response. Persistent elevation of celiac serologies triggers adherence investigation.

### Food Intolerance Evaluation Workflow

Patients with suspected food intolerance undergo structured evaluation. An elimination
diet DietaryIntervention is created with specific eliminated foods, a defined
elimination period, and a structured reintroduction protocol. Symptom response
during elimination and reintroduction guides the definitive dietary recommendation.

## Integration Points

- Upstream: receives nutritional concerns from Clinical Assessment Context.
- Upstream: receives dietary requirements from Inflammatory Disease and Hepatology Contexts.
- Downstream: communicates nutritional outcomes to Clinical Assessment for care planning.
- External: coordinates with dietary supply systems for enteral and parenteral products.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Cederholm, T. et al. (2019). "GLIM criteria for malnutrition." Clinical Nutrition.
- Rubio-Tapia, A. et al. (2013). "ACG Guidelines on celiac disease." American Journal of Gastroenterology.
- ASPEN Guidelines for enteral and parenteral nutrition (American Society for Parenteral and Enteral Nutrition).
