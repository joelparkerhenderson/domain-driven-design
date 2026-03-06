# Context Map in the Nutrition Domain

## Overview

A context map is a visual and conceptual representation of the relationships and interactions between bounded contexts in a system. In the nutrition domain, the context map documents how the six bounded contexts exchange information, which contexts are upstream or downstream, and what integration patterns govern their interactions. The context map serves as a strategic planning tool that guides architectural decisions and team coordination.

## Context Relationships

### Nutritional Assessment to Clinical Nutrition (Customer-Supplier)

The Nutritional Assessment context acts as the upstream supplier, providing assessment data including dietary analysis results, anthropometric measurements, and biomarker values. The Clinical Nutrition context is the downstream customer, consuming this data to formulate nutrition diagnoses and medical nutrition therapy plans. Clinical Nutrition can request specific assessment capabilities, and Nutritional Assessment commits to a published interface. This relationship follows the Customer-Supplier pattern because Clinical Nutrition has specific data needs that Assessment must satisfy.

### Nutritional Assessment to Sports Nutrition (Customer-Supplier)

Similarly, the Nutritional Assessment context supplies dietary and body composition data to the Sports Nutrition context. Sports Nutrition uses this data to calculate energy availability, assess nutritional adequacy relative to training demands, and identify micronutrient deficiencies common in athletes. The Sports Nutrition context may request assessment features specific to athletic populations, such as sweat rate analysis or exercise energy expenditure calculations.

### Meal Planning to Dietary Compliance (Upstream-Downstream)

The Meal Planning context publishes meal plans that the Dietary Compliance context uses as the reference standard for adherence tracking. Meal Planning is upstream, defining what should be eaten, while Dietary Compliance is downstream, recording what was actually eaten and comparing it against the plan. This relationship uses a Published Language pattern, with meal plans expressed in a shared schema that both contexts understand.

### Meal Planning to Clinical Nutrition (Conformist)

When meal plans must satisfy clinical dietary prescriptions, the Meal Planning context conforms to the constraints published by Clinical Nutrition. If Clinical Nutrition prescribes a renal diet with specific protein and potassium limits, Meal Planning adopts those constraints without translating them into its own model. This Conformist relationship reflects the regulatory authority of clinical prescriptions over meal design.

### Dietary Compliance to Outcomes Tracking (Upstream-Downstream)

The Dietary Compliance context publishes adherence data that flows downstream to Outcomes Tracking. Compliance scores, food log summaries, and barrier assessments become inputs for outcome analysis. Outcomes Tracking correlates dietary adherence patterns with health outcome trends to assess intervention effectiveness.

### All Contexts to Outcomes Tracking (Event-Based)

Outcomes Tracking receives domain events from all five other contexts. Assessment completion events, meal plan activations, clinical intervention changes, training phase transitions, and compliance score updates all flow into Outcomes Tracking through an event-driven architecture. This positions Outcomes Tracking as an aggregating downstream context.

### Clinical Nutrition to External EHR Systems (Anti-Corruption Layer)

Clinical Nutrition integrates with external electronic health record systems through an anti-corruption layer. The ACL translates HL7 FHIR resources, ICD diagnostic codes, and clinical order formats into the nutrition domain's own model, preventing external healthcare IT concepts from contaminating the domain model.

### Nutritional Assessment to Food Composition Databases (Open Host Service)

The Nutritional Assessment context exposes an Open Host Service that wraps external food composition databases such as USDA FoodData Central. This service translates raw food composition data into the domain's value objects, providing a stable interface even as underlying database versions change.

## Integration Patterns Summary

| Upstream Context | Downstream Context | Pattern |
|---|---|---|
| Nutritional Assessment | Clinical Nutrition | Customer-Supplier |
| Nutritional Assessment | Sports Nutrition | Customer-Supplier |
| Meal Planning | Dietary Compliance | Published Language |
| Clinical Nutrition | Meal Planning | Conformist |
| Dietary Compliance | Outcomes Tracking | Upstream-Downstream |
| All Contexts | Outcomes Tracking | Event-Based |
| External EHR | Clinical Nutrition | Anti-Corruption Layer |
| External Food DBs | Nutritional Assessment | Open Host Service |

## Evolution

The context map is a living document that evolves as the domain model matures. New integration points may emerge, relationships may shift from Conformist to Customer-Supplier as contexts gain independence, and new external system integrations may require additional anti-corruption layers.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on context maps.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 8 on context mapping patterns.
- Brandolini, Alberto. "Strategic Domain Driven Design with Context Mapping." InfoQ, 2009.
