# Repositories in the Nutrition Domain

## Overview

A repository is an abstraction that provides the illusion of an in-memory collection of aggregate roots, encapsulating the logic for storing, retrieving, and searching for domain objects. Repositories are defined as interfaces in the domain layer and implemented in the infrastructure layer, ensuring that business logic remains independent of persistence technology. In the nutrition domain, repositories serve each bounded context's aggregate roots, providing type-safe access to nutritional data while hiding storage details.

## Repository Interfaces by Bounded Context

### Nutritional Assessment Context

**NutritionalAssessmentRepository**
Provides access to NutritionalAssessment aggregates. Core operations include saving a new or updated assessment, finding an assessment by its identifier, finding all assessments for a given subject within a date range, and finding assessments with identified nutrient deficiencies. This repository supports the common query pattern of retrieving an individual's assessment history to track nutritional status over time.

**FoodItemRepository**
Provides access to FoodItem aggregates from the food composition database. Core operations include finding a food item by its identifier, searching food items by name or category, finding food items matching a specific nutrient criterion (e.g., foods high in iron), and retrieving food items by food group. This repository handles the large volume of reference data in food composition databases and supports efficient nutrient lookup during dietary analysis.

### Meal Planning Context

**MealPlanRepository**
Provides access to MealPlan aggregates. Core operations include saving a meal plan, finding a plan by its identifier, finding active plans for a given subject, and finding plans matching a specific dietary pattern. The repository supports the workflow of creating, activating, and archiving meal plans over time.

**RecipeRepository**
Provides access to Recipe aggregates. Core operations include saving a recipe, finding a recipe by its identifier, searching recipes by ingredient or nutrient profile, finding recipes matching dietary restrictions (e.g., gluten-free, dairy-free), and finding recipes within a caloric range. The recipe repository supports the meal planning workflow by enabling recipe discovery based on nutritional criteria.

### Clinical Nutrition Context

**NutritionCareRecordRepository**
Provides access to NutritionCareRecord aggregates. Core operations include saving a care record, finding a record by its identifier, finding records for a given patient, finding records with active nutrition diagnoses, and finding records requiring monitoring follow-up. This repository supports the Nutrition Care Process workflow by enabling clinicians to track the full lifecycle of nutritional interventions.

**DietPrescriptionRepository**
Provides access to DietPrescription aggregates. Core operations include saving a prescription, finding a prescription by its identifier, finding active prescriptions for a patient, and finding prescriptions by diet type. The repository supports the clinical workflow of creating, modifying, and discontinuing therapeutic dietary orders.

### Sports Nutrition Context

**PerformanceNutritionPlanRepository**
Provides access to PerformanceNutritionPlan aggregates. Core operations include saving a plan, finding a plan by its identifier, finding plans for a given athlete, finding plans in a specific training phase, and finding plans with flagged RED-S risk. This repository supports the periodized nutrition planning workflow for athletic populations.

### Dietary Compliance Context

**ComplianceRecordRepository**
Provides access to ComplianceRecord aggregates. Core operations include saving a compliance record, finding a record by its identifier, finding records for a given subject within a date range, finding records with compliance scores below a threshold, and finding records with identified barriers. This repository supports the compliance tracking workflow by enabling retrieval and analysis of adherence data.

### Outcomes Tracking Context

**OutcomeTimelineRepository**
Provides access to OutcomeTimeline aggregates. Core operations include saving a timeline, finding a timeline by its identifier, finding the timeline for a given subject, and querying data points by metric type and date range. This repository supports the longitudinal outcome analysis workflow by providing efficient access to time-series health data.

## Repository Design Principles

### Collection-Oriented vs. Persistence-Oriented

Repositories in the nutrition domain follow the collection-oriented style described by Evans, presenting aggregates as if they were held in an in-memory collection. The repository interface expresses operations in domain terms (findAssessmentsWithDeficiencies) rather than persistence terms (executeQuery). This keeps the domain layer free of infrastructure concerns.

### Query Specifications

For complex query scenarios, repositories accept specification objects that encode domain-specific search criteria. For example, a NutrientDeficiencySpecification might encode "find all assessments where vitamin D intake is below 600 IU/day and serum 25-hydroxyvitamin D is below 20 ng/mL." Specifications keep query logic in the domain layer rather than embedding it in repository implementations.

### No Cross-Aggregate Queries

Repositories retrieve only their own aggregate type. A query that spans multiple aggregate types (e.g., "find all patients with active diagnoses and low compliance scores") is handled by a domain service that coordinates across multiple repositories, preserving aggregate boundaries.

### Interface in Domain, Implementation in Infrastructure

Repository interfaces are defined in the domain layer using domain types only. Implementation classes in the infrastructure layer handle database connections, ORM mappings, caching strategies, and query optimization. This separation allows the domain model to remain pure and testable without infrastructure dependencies.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 6 on repositories.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 12 on repositories.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on persistence of aggregates.
