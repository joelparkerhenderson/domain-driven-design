# Anti-Corruption Layer in the Nutrition Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that prevents external system concepts, data formats, and models from contaminating a bounded context's domain model. In the nutrition domain, ACLs are essential because the system must integrate with multiple external systems, each carrying its own conceptual model that differs from the nutrition domain's ubiquitous language. Without ACLs, the domain model would degrade into a patchwork of foreign concepts that no longer reflects the domain experts' understanding.

## External System Integrations Requiring ACLs

### Electronic Health Record (EHR) Systems

The Clinical Nutrition context integrates with EHR systems that use HL7 FHIR resources, ICD-10 diagnostic codes, and clinical order entry formats. An ACL translates between EHR concepts and the nutrition domain model. For example, an EHR "NutritionOrder" FHIR resource maps to the domain's DietPrescription entity, but the translation must account for differences in how the two systems represent dietary modifications, nutrient restrictions, and oral diet preferences. The ACL ensures that FHIR-specific structures like "oralDiet.nutrient.modifier" do not leak into the domain model.

### Food Composition Databases

The Nutritional Assessment context relies on external food composition databases such as USDA FoodData Central, which provides nutrient data for thousands of food items. The ACL translates the database's flat nutrient records, food description codes (NDB numbers), and data type classifications into the domain's FoodItem value objects and NutrientProfile aggregates. When the USDA changes its data format or nutrient coding scheme between versions (such as the transition from Standard Reference to FoodData Central), the ACL absorbs the change without requiring modifications to the domain model.

### Laboratory Information Systems

Biomarker data flows from laboratory information systems into the Nutritional Assessment context. Lab systems report results using LOINC codes, reference ranges, and units that differ from how the nutrition domain models biomarker values. The ACL translates LOINC-coded lab results into the domain's Biomarker value objects, mapping lab-specific terminology (such as "25-Hydroxyvitamin D" with a specific LOINC code) to the domain's "Vitamin D Status" concept.

### Fitness and Wearable Device Platforms

The Sports Nutrition context may receive data from fitness tracking platforms and wearable devices. These platforms report exercise data, heart rate, sleep metrics, and estimated caloric expenditure in their own proprietary formats. The ACL translates this data into the domain's ExerciseSession entities and EnergyExpenditure value objects, filtering out fitness-platform-specific concepts like "activity rings" or "training effect scores" that have no meaning in the nutrition domain.

### Supplement and Pharmaceutical Databases

The Sports Nutrition and Clinical Nutrition contexts may reference external supplement and drug databases for interaction checking and evidence-based recommendations. The ACL translates external product identifiers, ingredient lists, and interaction warnings into the domain's Supplement and InteractionRisk value objects.

## ACL Design Principles

### Isolation

The ACL operates at the boundary of the bounded context, completely isolating the domain model from external system details. Domain entities and value objects never reference external data types, identifiers, or formats directly.

### Translation

The ACL performs bidirectional translation. Inbound translation converts external data into domain objects. Outbound translation converts domain objects into external formats when the nutrition domain must push data to external systems.

### Adaptation to Change

External systems change independently of the nutrition domain. The ACL absorbs these changes, providing a stable interface to the domain model even as external APIs, data formats, and coding schemes evolve. This is particularly important for food composition databases that release periodic updates.

### Validation and Enrichment

During translation, the ACL validates incoming data against domain rules and may enrich it with domain-specific defaults. For example, if a lab result arrives without a reference range, the ACL may apply the domain's standard reference ranges for nutritional biomarkers.

## Implementation Considerations

ACLs in the nutrition domain typically consist of adapter classes that implement domain-defined interfaces, translator services that map between external and domain models, and gateway objects that handle communication protocols. The ACL should be tested against both the external system's actual data formats and the domain model's invariants to ensure translation correctness.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 13 on integrating bounded contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on integration patterns.
- HL7 International. FHIR NutritionOrder Resource. https://www.hl7.org/fhir/nutritionorder.html
