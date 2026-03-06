# Nutrition Domain - CLAUDE.md

## Overview

This domain applies Domain-Driven Design (DDD) to nutrition management systems, encompassing nutritional assessment, meal planning, clinical nutrition, sports nutrition, dietary compliance, and outcomes tracking.

## Bounded Contexts

1. Nutritional Assessment Context - dietary recall, anthropometry, lab-based biomarkers, nutrient analysis
2. Meal Planning Context - meal design, recipe management, grocery lists, macro/micronutrient targets
3. Clinical Nutrition Context - medical nutrition therapy, enteral/parenteral nutrition, disease-specific diets
4. Sports Nutrition Context - performance nutrition, hydration protocols, supplementation, periodized fueling
5. Dietary Compliance Context - food logging, adherence tracking, behavioral coaching, barrier identification
6. Outcomes Tracking Context - weight management, biomarker trends, body composition, patient-reported outcomes

## Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- nutritional-assessment-context
- meal-planning-context
- clinical-nutrition-context
- sports-nutrition-context
- dietary-compliance-context
- outcomes-tracking-context
- event-driven-architecture
- integration-patterns
- nutrition-standards
- regulatory-compliance

## Design Principles

- Business logic is pure and independent of infrastructure concerns.
- Each bounded context owns its own ubiquitous language and domain model.
- Communication between contexts uses domain events and integration patterns.
- Nutrition-specific standards (DRIs, food composition databases) inform value object design.
- Regulatory compliance (HIPAA, FDA labeling, USDA guidelines) shapes model constraints.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
