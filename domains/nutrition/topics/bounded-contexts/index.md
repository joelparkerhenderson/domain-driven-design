# Bounded Contexts in the Nutrition Domain

## Overview

A bounded context is a logical boundary within which a specific domain model is defined and consistent. In the nutrition domain, six bounded contexts reflect the natural divisions of nutritional practice, each with its own ubiquitous language, business rules, and domain model. These boundaries prevent model corruption that would arise from forcing a single unified model across the diverse concerns of nutritional assessment, clinical care, athletic performance, and behavioral compliance.

## The Six Bounded Contexts

### 1. Nutritional Assessment Context

This context is responsible for capturing and analyzing an individual's nutritional status. It handles dietary recall methods (24-hour recall, food frequency questionnaires, food diaries), anthropometric measurements (height, weight, BMI, waist circumference, skinfold thickness), laboratory biomarkers (serum albumin, prealbumin, hemoglobin, micronutrient levels), and nutrient analysis against Dietary Reference Intakes. The core aggregate is the NutritionalAssessment, which brings together intake data, body measurements, and lab results into a comprehensive nutritional status profile.

### 2. Meal Planning Context

This context manages the design and delivery of structured eating plans. It encompasses recipe management with ingredient lists and nutritional breakdowns, meal plan creation targeting specific macro and micronutrient profiles, grocery list generation from meal plans, and dietary pattern customization. The core aggregate is the MealPlan, which orchestrates recipes, nutrient targets, and scheduling into a coherent eating strategy. The term "meal" in this context refers to a planned combination of recipes, not a consumed food event.

### 3. Clinical Nutrition Context

This context addresses medical nutrition therapy and therapeutic dietary interventions. It manages nutrition diagnoses using the International Dietetics and Nutrition Terminology (IDNT), medical nutrition therapy protocols for chronic diseases, enteral and parenteral nutrition orders, and disease-specific diet prescriptions. The core aggregate is the NutritionCareRecord, which tracks the full Nutrition Care Process from assessment through monitoring. Clinical Nutrition operates under strict regulatory and professional practice constraints.

### 4. Sports Nutrition Context

This context focuses on nutrition strategies that support athletic performance. It covers periodized fueling plans aligned with training cycles, hydration protocols for training and competition, supplementation regimens with evidence-based recommendations, and energy availability monitoring for RED-S prevention. The core aggregate is the PerformanceNutritionPlan, which integrates fueling strategy, hydration, and supplementation within the context of an athlete's training periodization.

### 5. Dietary Compliance Context

This context tracks how well an individual follows their dietary plan. It manages food log entries with portion estimation and meal timing, adherence scoring against prescribed dietary plans, behavioral coaching interventions and motivational strategies, and barrier identification and resolution. The core aggregate is the ComplianceRecord, which correlates logged intake against planned intake to calculate adherence metrics and trigger coaching interventions.

### 6. Outcomes Tracking Context

This context monitors the results of nutritional interventions over time. It handles weight management trajectories and goal tracking, biomarker trend analysis for clinical decision support, body composition assessment (lean mass, fat mass, bone density), and patient-reported outcomes such as energy levels, satiety, and quality of life. The core aggregate is the OutcomeTimeline, which assembles data points from multiple sources into longitudinal trend views.

## Context Boundaries and Language

Each bounded context defines its own interpretation of shared terms. For example, "intake" in the Assessment context means reported food consumption data, while in the Clinical context it refers to a calculated nutrient delivery from enteral or parenteral formulas. Similarly, "target" means a DRI-based nutrient goal in Assessment, a meal plan macro ratio in Meal Planning, and a therapeutic nutrient limit in Clinical Nutrition. These distinctions justify separate models rather than a single shared model.

## Ownership and Team Alignment

Following Conway's Law and DDD best practices, each bounded context should be owned by a team with the relevant domain expertise. The Clinical Nutrition context requires team members with clinical dietetics knowledge. The Sports Nutrition context benefits from exercise science expertise. This alignment ensures the model remains faithful to the domain reality.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 6 on bounded contexts in practice.
