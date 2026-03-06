# Integration Patterns in the Nutrition Domain

## Overview

Integration patterns define how bounded contexts interact, share data, and maintain their model integrity during collaboration. In Domain-Driven Design, these patterns describe the organizational and technical relationships between contexts, ranging from tightly coupled shared models to fully independent contexts communicating only through events. The nutrition domain employs multiple integration patterns to manage the complex relationships between its six bounded contexts and external systems.

## Patterns Applied in the Nutrition Domain

### Customer-Supplier

The Customer-Supplier pattern establishes a relationship where the downstream context (customer) can request features and data formats from the upstream context (supplier), and the supplier commits to supporting those needs.

**Nutritional Assessment to Clinical Nutrition.** Clinical Nutrition is the customer, requiring specific assessment data formats, nutrient deficiency classifications, and biomarker interpretations. Nutritional Assessment is the supplier, committing to a stable interface that delivers assessment results in the format Clinical Nutrition needs. Clinical Nutrition can request new assessment capabilities (such as a new screening tool) through the backlog negotiation process.

**Nutritional Assessment to Sports Nutrition.** Sports Nutrition is the customer, requiring assessment data tailored to athletic populations, such as energy expenditure calculations and body composition measurements optimized for athletes. Nutritional Assessment supplies this data and accommodates sport-specific assessment requests.

### Published Language

The Published Language pattern uses a well-documented, shared data format for communication between contexts, ensuring both sides understand the exchanged information without ambiguity.

**Meal Planning to Dietary Compliance.** Meal plans are communicated from Meal Planning to Dietary Compliance using a published meal plan schema that defines the structure for daily meal schedules, recipes, portion sizes, and nutritional targets. Both contexts agree on this schema, enabling Dietary Compliance to accurately compare logged intake against planned intake. The published language includes definitions for meal occasions, serving size units, and nutrient target formats.

### Conformist

The Conformist pattern exists when a downstream context adopts the upstream context's model without requesting changes, accepting the upstream model as given.

**Clinical Nutrition to Meal Planning.** When Clinical Nutrition issues a diet prescription, Meal Planning conforms to the prescription constraints without translating them into its own model. The protein restriction of 0.8g/kg/day for a renal patient is used directly as a Meal Planning constraint. This conformist relationship reflects the clinical authority of the prescription: Meal Planning does not reinterpret clinical dietary orders but applies them as specified.

### Anti-Corruption Layer

The Anti-Corruption Layer (ACL) pattern places a translation boundary between a bounded context and an external system whose model would otherwise contaminate the domain.

**Clinical Nutrition to EHR Systems.** An ACL translates between HL7 FHIR resources (NutritionOrder, Condition, Observation) and the Clinical Nutrition context's domain model. The ACL converts ICD-10 codes to nutrition-relevant condition categories, maps FHIR NutritionOrder structures to the domain's DietPrescription aggregate, and translates clinical observations into the domain's Biomarker value objects.

**Nutritional Assessment to Food Composition Databases.** An ACL translates USDA FoodData Central data formats, NDB numbers, and nutrient coding schemes into the domain's FoodItem aggregates and NutrientProfile value objects. The ACL absorbs version changes in the external database.

### Open Host Service

The Open Host Service pattern exposes a context's capabilities through a well-defined protocol that external consumers can use without special integration.

**Nutritional Assessment as Open Host.** The Nutritional Assessment context exposes an Open Host Service that wraps access to the food composition database, providing a stable API for food item lookup and nutrient analysis. Other contexts and external systems can use this service without needing to understand the underlying food composition database format.

### Shared Kernel

The Shared Kernel pattern is used sparingly in the nutrition domain. A small shared kernel exists for fundamental nutritional concepts that are identical across multiple contexts: the NutrientType enumeration (listing all tracked nutrients), unit conversion logic (grams to milligrams, ounces to grams), and the DRI reference value set. This shared kernel is versioned and changes require agreement from all consuming contexts.

### Separate Ways

Some aspects of the nutrition domain deliberately follow the Separate Ways pattern, where contexts do not integrate at all. For example, the recipe management workflow within Meal Planning and the food logging workflow within Dietary Compliance operate independently. While both contexts deal with food items, they manage their food references separately because recipes and food logs serve fundamentally different purposes.

## Integration Governance

Integration patterns in the nutrition domain are documented in the context map, reviewed during architecture decisions, and governed by cross-team agreements. Changes to integration contracts (published languages, shared kernels, anti-corruption layer mappings) require coordination between the teams owning the affected contexts. Version management of shared schemas prevents breaking changes from propagating across context boundaries.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9 on integration patterns.
