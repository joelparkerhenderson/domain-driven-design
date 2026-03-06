# Sports Nutrition Context

## Overview

The Sports Nutrition Context focuses on nutrition strategies that support athletic performance, recovery, and long-term health in physically active populations. It designs periodized fueling strategies aligned with training cycles, manages hydration protocols for training and competition environments, tracks supplementation regimens with evidence-based recommendations, and monitors energy availability to prevent relative energy deficiency in sport (RED-S). This context bridges exercise physiology and nutritional science.

This bounded context is classified as a supporting subdomain. It provides specialized value for athletic populations with unique nutritional demands that differ substantially from general or clinical nutrition. The periodization concept, where nutritional needs shift with training phases, introduces domain complexity not found in other contexts.

## Ubiquitous Language

Within this context, "periodization" refers to the systematic variation of training and nutrition across phases (base, build, peak, taper, competition, recovery). "Fueling" means the strategic provision of energy and macronutrients to support training adaptation and performance. "Recovery" specifically means post-exercise nutritional restoration, not general health recovery. "Availability" refers to energy availability, the energy remaining for physiological functions after exercise cost. "Protocol" means a structured, prescriptive plan for hydration or supplementation.

## Aggregates

**PerformanceNutritionPlan** is the primary aggregate root. It contains FuelingPhase entities (each aligned with a training periodization phase), HydrationProtocol value objects, and SupplementationSchedule entities. The aggregate enforces that energy availability remains above safe thresholds across all phases, that supplementation does not include substances on prohibited lists (for competitive athletes), and that each phase has defined macronutrient targets.

## Entities

**FuelingPhase** aligns specific nutrition targets with a training phase. Each phase has unique carbohydrate targets (expressed as g/kg body weight, varying from 3-5 g/kg for light training to 8-12 g/kg for extreme endurance), protein targets (1.2-2.0 g/kg depending on sport type and phase), fat targets, and meal timing recommendations around training sessions. Phases are individually tracked because their nutritional parameters change as the athlete transitions through the periodization cycle.

**SupplementationSchedule** defines a specific supplement within the athlete's regimen, including the supplement identity (e.g., creatine monohydrate, caffeine, beta-alanine, vitamin D), dosage, timing relative to training, cycle duration, and the evidence classification supporting its use. Each supplement is tracked individually for compliance monitoring and adverse effect reporting.

## Value Objects

**EnergyAvailability** represents dietary energy minus exercise energy expenditure, normalized per kilogram of fat-free mass. Values below 30 kcal/kg FFM/day indicate RED-S risk; below 20 kcal/kg FFM/day indicates high risk. **HydrationTarget** specifies fluid type, volume, and timing relative to exercise. **SweatRate** captures fluid loss per hour of exercise under specific conditions, used to individualize hydration protocols. **TrainingLoad** quantifies exercise demand for energy expenditure calculations.

## Domain Events

**FuelingPhaseTransitioned** is raised when an athlete moves to a new training phase with different nutritional requirements. This event notifies Meal Planning to adjust the athlete's eating plan and Dietary Compliance to update adherence targets. **REDSRiskDetected** is raised when energy availability drops below safe thresholds, triggering clinical evaluation through the Clinical Nutrition context and flagging in Outcomes Tracking. **HydrationProtocolUpdated** notifies compliance tracking of new hydration targets.

## Domain Services

**EnergyAvailabilityCalculationService** calculates energy availability by integrating dietary intake data, exercise energy expenditure, and body composition (fat-free mass). This service spans data from multiple sources and produces the critical EA value object with risk classification.

**PeriodizedFuelingService** generates phase-specific macronutrient targets based on the athlete's training plan, body composition, sport type, and performance goals. It applies sport-specific guidelines (endurance vs. strength vs. team sport) to produce individualized fueling recommendations.

**SupplementEvidenceService** evaluates supplements against current evidence classifications (Category A: strong evidence, Category B: emerging evidence, Category C: limited evidence, Category D: prohibited or harmful) based on frameworks from organizations like the Australian Institute of Sport.

## Repositories

**PerformanceNutritionPlanRepository** provides access to plans with operations for finding by athlete, current training phase, and RED-S risk status.

## Integration Points

This context is downstream of Nutritional Assessment, receiving assessment data to inform fueling strategy adjustments. It publishes FuelingPhaseTransitioned events to Meal Planning and REDSRiskDetected events to Clinical Nutrition. It may integrate with external training management platforms through an anti-corruption layer to receive training load and periodization data.

## Sport-Specific Considerations

Different sports impose different nutritional demands. Endurance athletes require higher carbohydrate availability and careful glycogen loading protocols. Strength and power athletes prioritize protein timing and creatine supplementation. Weight-category athletes face additional challenges around making weight safely without compromising energy availability. Team sport athletes need strategies that account for match schedules, travel, and variable training loads. The domain model accommodates these variations through configurable sport profiles within the PerformanceNutritionPlan aggregate.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Thomas, D. Travis, et al. "Position of the Academy of Nutrition and Dietetics, Dietitians of Canada, and the American College of Sports Medicine: Nutrition and Athletic Performance." Journal of the Academy of Nutrition and Dietetics, 2016.
- Mountjoy, Margo, et al. "IOC Consensus Statement on Relative Energy Deficiency in Sport (RED-S)." British Journal of Sports Medicine, 2018.
