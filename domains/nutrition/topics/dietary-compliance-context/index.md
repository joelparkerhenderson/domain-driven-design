# Dietary Compliance Context

## Overview

The Dietary Compliance Context tracks how closely an individual follows their prescribed or recommended dietary plan. It manages food logging with detailed portion estimation and meal timing, calculates adherence scores by comparing logged intake against planned intake, delivers behavioral coaching interventions to improve compliance, and identifies barriers that prevent individuals from following dietary recommendations. This context sits at the intersection of nutrition science and behavioral psychology, recognizing that nutritional outcomes depend not only on what is prescribed but on what is actually consumed.

This bounded context is classified as a supporting subdomain. Compliance tracking and behavioral coaching enhance the effectiveness of core nutrition interventions but rely on established behavioral science principles and food logging methodologies rather than requiring novel domain innovation.

## Ubiquitous Language

Within this context, "log" refers to the act of recording food consumption with item, quantity, and timing details. "Adherence" and "compliance" are used interchangeably to mean the degree of alignment between actual and prescribed intake. "Barrier" is an identified obstacle preventing dietary adherence. "Coaching" means a structured behavioral intervention aimed at improving compliance. "Occasion" refers to the meal context (breakfast, lunch, dinner, snack, pre-workout, post-workout) in which food was consumed.

## Aggregates

**ComplianceRecord** is the primary aggregate root. It tracks dietary adherence over a reporting period (typically weekly). It contains FoodLogEntry entities recording each consumed item, a reference to the active MealPlan (by identifier, from the Meal Planning context), a calculated ComplianceScore value object, and BarrierAssessment value objects identifying obstacles. The aggregate enforces that compliance scores are recalculated when new log entries are added and that barrier assessments reference specific periods of non-compliance.

## Entities

**FoodLogEntry** records a single food or beverage consumption event. Each entry has a unique identifier, a timestamp, a food item reference (by identifier), a consumed quantity with estimation method, a meal occasion classification, and optional notes (e.g., dining context, emotional state). Individual entries are tracked separately because they form the granular data from which adherence calculations are derived.

**CoachingIntervention** represents a behavioral coaching action delivered to the individual. Each intervention has a unique identifier, a type (motivational interviewing, goal setting, stimulus control, cognitive restructuring, self-monitoring feedback), a delivery date, a target barrier, and an outcome assessment. Interventions are tracked individually to evaluate which coaching strategies are most effective.

## Value Objects

**ComplianceScore** represents calculated adherence as a percentage with component scores: macronutrient compliance (how closely actual macros matched targets), micronutrient compliance, meal timing compliance (whether meals were consumed at planned times), and overall compliance. The score is immutable once calculated for a period.

**PortionEstimate** captures the method used to estimate a food portion: digital scale (highest accuracy), measuring cups/spoons, visual estimate, hand-based portions (palm for protein, fist for vegetables), or photograph-based estimation. It includes a confidence level reflecting the estimation method's typical accuracy.

**BarrierAssessment** identifies a specific obstacle to dietary adherence with a barrier type (time constraints, cost, taste preferences, social pressure, knowledge gaps, access limitations, emotional eating), supporting evidence from compliance patterns, and suggested intervention strategies.

## Domain Events

**ComplianceScoreCalculated** is raised when adherence is computed for a reporting period. It carries the subject identifier, period, overall percentage, and component scores. Consumed by Outcomes Tracking to build adherence trend data and by Clinical Nutrition to inform therapy adjustments for patients with persistently low compliance.

**ComplianceBarrierIdentified** is raised when pattern analysis detects a systematic barrier (e.g., consistent evening overeating or weekend non-compliance). It carries the barrier type, pattern evidence, and suggested interventions. Consumed by Clinical Nutrition for counseling purposes.

**FoodLogStreakBroken** is raised when an individual fails to log food for a specified consecutive period, indicating potential disengagement from the compliance tracking process.

## Domain Services

**AdherenceScoringService** calculates compliance by comparing food log entries against the active meal plan. It matches logged foods to planned foods using fuzzy matching (accounting for reasonable substitutions), evaluates portion accuracy within configurable tolerance bands, assesses meal timing adherence, and computes component and aggregate scores.

**BarrierAnalysisService** analyzes patterns in compliance data to identify systematic barriers. It applies temporal analysis (identifying days of week or times of day with consistently low compliance), situational analysis (correlating non-compliance with contextual factors like travel or social events), and nutritional analysis (identifying specific nutrient targets that are consistently missed).

## Repositories

**ComplianceRecordRepository** provides access to compliance records with operations for finding by subject, date range, compliance score thresholds, and identified barrier types.

## Integration Points

This context is downstream of Meal Planning, receiving activated meal plans as the reference standard for adherence calculations (Published Language pattern). It publishes ComplianceScoreCalculated events to Outcomes Tracking and Clinical Nutrition. It receives DietPrescriptionIssued events from Clinical Nutrition to update compliance targets when therapeutic diets change. It receives FuelingPhaseTransitioned events from Sports Nutrition to adjust adherence criteria for athletes.

## Behavioral Science Foundation

The compliance context draws on established behavioral science frameworks including the Transtheoretical Model (stages of change), Social Cognitive Theory (self-efficacy), and the Health Belief Model. Coaching interventions are structured around these frameworks, and barrier identification uses categories validated in dietary adherence research. The domain model does not implement these theories directly but provides the data structures and events needed to support evidence-based behavioral interventions.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Spahn, Joanne M., et al. "State of the Evidence Regarding Behavior Change Theories and Strategies in Nutrition Counseling." Journal of the American Dietetic Association, 2010.
