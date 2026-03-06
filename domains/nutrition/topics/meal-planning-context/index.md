# Meal Planning Context

## Overview

The Meal Planning Context is responsible for designing structured eating plans that translate nutritional targets into actionable meals. It manages recipes with ingredient lists and calculated nutritional breakdowns, organizes meals into plans covering defined time periods, generates grocery lists from plan ingredients, and supports customization for diverse dietary patterns. This context serves as the bridge between nutritional science (what nutrients are needed) and daily eating behavior (what foods to consume and when).

This bounded context is classified as a supporting subdomain. While essential to the nutrition domain, meal planning mechanics are well-understood and can leverage existing recipe databases and planning algorithms. The domain-specific value comes from integrating meal design with nutritional targets, dietary restrictions, and clinical prescriptions from other contexts.

## Ubiquitous Language

Within this context, "meal" refers to a planned combination of recipes assigned to a meal occasion (breakfast, lunch, dinner, snack), not a consumed food event. "Plan" is a structured schedule of meals over a defined period. "Target" means a macronutrient or micronutrient goal that the plan should achieve. "Pattern" refers to a named dietary style (Mediterranean, DASH, ketogenic, plant-based) that shapes recipe selection and meal composition.

## Aggregates

**MealPlan** is the primary aggregate root. It contains PlannedMeal entities organized by day and meal occasion, references to Recipe aggregates through identifiers, and MacronutrientRatio and micronutrient target value objects. The MealPlan enforces invariants: daily caloric totals must fall within a specified range, macronutrient ratios must sum to 100%, and all days must have at least three planned meals.

**Recipe** is a separate aggregate root containing Ingredient entities (each referencing a FoodItem identifier with a quantity and unit), preparation instructions, cooking time, yield information, and a calculated NutritionalBreakdown value object. Recipes are independent aggregates because they are reusable across multiple meal plans and have their own creation and modification lifecycle.

## Entities

**PlannedMeal** is an entity within the MealPlan aggregate, identified by its position (day number and meal occasion). It references one or more recipes with portion multipliers and allows substitution notes. Each PlannedMeal carries its own calculated nutritional subtotal.

**Ingredient** is an entity within the Recipe aggregate, referencing a FoodItem by identifier and specifying a quantity, unit, and optional preparation notes (e.g., "chopped," "raw," "cooked"). The same FoodItem may appear multiple times in a recipe as different ingredients with different preparations.

## Value Objects

**MacronutrientRatio** represents the target distribution of calories from protein, carbohydrate, and fat as percentages that sum to 100. **ServingSize** encapsulates a food portion with quantity, unit, and household description. **NutritionalBreakdown** aggregates all nutrient amounts for a recipe or meal per serving. **DietaryRestriction** captures a food exclusion (allergen, intolerance, preference) with its type and specific items.

## Domain Events

**MealPlanActivated** is raised when a plan transitions from draft to active, notifying Dietary Compliance to establish the adherence reference and Outcomes Tracking to record the intervention start. **MealPlanModified** is raised when an active plan changes, updating compliance reference data. **GroceryListGenerated** is raised when ingredients are aggregated into a shopping list.

## Domain Services

**MealPlanOptimizationService** generates or adjusts meal plans to meet nutritional targets while respecting dietary preferences, allergies, budget constraints, and recipe availability. This is a cross-aggregate operation spanning MealPlan and multiple Recipe aggregates. **GroceryListAggregationService** consolidates ingredients from all recipes in a meal plan into a unified shopping list, performing unit conversions and quantity aggregation.

## Repositories

**MealPlanRepository** provides access to MealPlan aggregates with operations for finding by subject, status (draft, active, archived), and dietary pattern. **RecipeRepository** provides access to Recipe aggregates with search capabilities by ingredient, nutrient profile, dietary restriction compatibility, caloric range, and preparation time.

## Integration Points

This context is downstream of Clinical Nutrition, conforming to diet prescriptions that constrain meal plan design. It is upstream of Dietary Compliance, publishing activated meal plans as adherence references. It receives NutrientDeficiencyIdentified events from Nutritional Assessment to adjust nutrient targets. It receives FuelingPhaseTransitioned events from Sports Nutrition to adapt plans for athletic populations.

## Dietary Pattern Support

The context supports multiple dietary patterns through configurable recipe filtering and nutrient target profiles. Mediterranean patterns emphasize olive oil, fish, whole grains, and produce. DASH patterns restrict sodium and emphasize potassium-rich foods. Ketogenic patterns set very low carbohydrate targets. Plant-based patterns exclude animal products. Each pattern is modeled as a combination of nutrient targets and food category preferences rather than a rigid template.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Mahan, L. Kathleen, and Janice L. Raymond. Krause and Mahan's Food and the Nutrition Care Process. Elsevier, 2020.
