# Clinical Nutrition Context

## Overview

The Clinical Nutrition Context addresses medical nutrition therapy and therapeutic dietary interventions for patients with acute or chronic medical conditions. It implements the Nutrition Care Process standardized by the Academy of Nutrition and Dietetics, manages nutrition diagnoses using the International Dietetics and Nutrition Terminology (IDNT), prescribes disease-specific diets, and oversees enteral and parenteral nutrition delivery. This context operates under strict clinical governance, regulatory compliance, and professional practice standards.

This bounded context is classified as a core subdomain because it contains the most complex and safety-critical domain logic in the nutrition system. Errors in clinical nutrition can directly harm patients, making domain accuracy paramount. The interplay of disease pathophysiology, drug-nutrient interactions, and individualized therapeutic targets creates rich business logic that requires deep clinical expertise.

## Ubiquitous Language

Within this context, "diagnosis" means a formal nutrition problem identification using IDNT, not a medical diagnosis. "Therapy" refers to Medical Nutrition Therapy (MNT), the evidence-based application of dietary intervention to treat medical conditions. "Order" carries clinical authority, meaning a formally prescribed nutritional intervention. "Monitoring" means the systematic reassessment of nutrition indicators to evaluate intervention effectiveness. "Tolerance" refers to a patient's physiological acceptance of a feeding regimen.

## Aggregates

**NutritionCareRecord** is the primary aggregate root implementing the full Nutrition Care Process for a patient. It contains NutritionDiagnosis entities (each with a PES statement), NutritionIntervention entities (diet prescriptions, education plans, supplement orders), and MonitoringEvaluation value objects documenting reassessment findings. The aggregate enforces that every intervention links to at least one active diagnosis and that interventions cannot be closed without a monitoring evaluation.

**DietPrescription** is a separate aggregate root defining a therapeutic dietary order. It contains NutrientRestriction value objects (e.g., protein restricted to 0.8g/kg/day for chronic kidney disease), AllowedFoodCategory entities, and MealTimingRequirement value objects. Diet prescriptions have their own approval lifecycle and may be referenced by multiple care records.

## Entities

**NutritionDiagnosis** is identified uniquely and carries a PES statement (Problem-Etiology-Signs/Symptoms), a status (active, resolved, inactive), a date of identification, and links to supporting assessment data. The diagnosis identity persists as its status changes through the care process.

**NutritionIntervention** represents a specific therapeutic action: dietary modification, nutrition education, care coordination, or supplement/formula order. Each intervention tracks its implementation status, the responsible clinician, and links to the diagnoses it addresses.

**EnteralFeedingOrder** specifies a tube feeding regimen including formula product, delivery method (continuous, bolus, cyclic), rate, volume target, and schedule. The order has a lifecycle from prescription through administration and monitoring.

## Value Objects

**PESStatement** encapsulates the standardized diagnostic format. **NutrientRestriction** represents a therapeutic nutrient limit with nutrient type, restriction direction (maximum, minimum, range), and values. **FormulaComposition** describes enteral or parenteral formula nutritional content per unit volume. **MonitoringEvaluation** captures reassessment findings with indicator values, comparison to targets, and clinical judgment.

## Domain Events

**NutritionDiagnosisEstablished** notifies downstream contexts of a new clinical finding. **DietPrescriptionIssued** triggers Meal Planning to apply new constraints and Dietary Compliance to update adherence targets. **EnteralFeedingStarted** notifies Outcomes Tracking and Compliance of the intervention initiation. **TherapyGoalAchieved** signals that a clinical nutrition target has been met.

## Domain Services

**NutritionDiagnosisService** assists in formulating diagnoses by analyzing assessment data against clinical criteria. **EnteralFormulaSelectionService** recommends formulas based on clinical condition and nutrient requirements. **NutrientDrugInteractionService** checks for interactions between dietary intake and medications.

## Repositories

**NutritionCareRecordRepository** supports finding records by patient, active diagnosis status, and monitoring follow-up needs. **DietPrescriptionRepository** supports finding prescriptions by patient, diet type, and active status.

## Integration Points

This context receives AssessmentCompleted events from Nutritional Assessment to inform diagnosis workflows. It publishes DietPrescriptionIssued events consumed by Meal Planning (Conformist relationship). It receives REDSRiskDetected events from Sports Nutrition for athletic patients. It integrates with external EHR systems through an anti-corruption layer that translates HL7 FHIR resources and ICD codes.

## Regulatory and Professional Standards

Clinical nutrition practice must comply with healthcare regulations including HIPAA for patient data protection, state scope-of-practice laws for registered dietitians, and institutional clinical governance policies. Documentation must meet standards for medical record keeping, and nutrition diagnoses must use standardized IDNT terminology to ensure clinical communication accuracy.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Academy of Nutrition and Dietetics. International Dietetics and Nutrition Terminology (IDNT) Reference Manual. 2020.
- Mahan, L. Kathleen, and Janice L. Raymond. Krause and Mahan's Food and the Nutrition Care Process. Elsevier, 2020.
