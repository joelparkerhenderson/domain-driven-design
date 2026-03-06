# Stone Disease Context

## Overview

The Stone Disease Context manages the evaluation, treatment, and prevention of nephrolithiasis (kidney stones). This bounded context addresses the complete stone disease lifecycle from acute presentation and stone characterization through procedural intervention and long-term metabolic prevention. It owns the clinical models for stone composition, metabolic risk assessment, treatment selection algorithms, and recurrence prevention strategies.

## Scope

This context covers acute stone episode management, stone imaging and characterization (CT stone protocol, ultrasound), stone composition analysis (calcium oxalate monohydrate, calcium oxalate dihydrate, calcium phosphate, uric acid, struvite, cystine), metabolic evaluation (24-hour urine collections, serum chemistry), treatment selection (observation, medical expulsive therapy, ESWL, ureteroscopy, PCNL), and long-term metabolic prevention (dietary modification, pharmacologic therapy, recurrence monitoring).

## Ubiquitous Language

"Stone burden" quantifies the total volume of calculi present. "Hounsfield units" measure stone density on CT, predicting composition and ESWL fragility. "Stone-free rate" defines treatment success as absence of residual fragments on imaging. "Metabolic workup" refers specifically to 24-hour urine collection analysis with supersaturation calculations. "Recurrence" is a new stone formation event in a patient with prior nephrolithiasis. "Medical expulsive therapy" is pharmacologic treatment (alpha-blockers) to facilitate spontaneous passage of ureteral stones.

## Aggregates

The StoneEpisode aggregate root manages a single nephrolithiasis event from presentation through resolution. It contains StoneCharacterization value objects, TreatmentDecision entities, and ProcedureOutcome entities. The MetabolicProfile aggregate root manages a patient's metabolic risk factors for stone formation, containing TwentyFourHourUrineResult value objects, SerumChemistry value objects, and DietaryAssessment entities.

## Key Entities

StoneEvent entities track individual stone episodes with presentation details, imaging findings, and clinical course. TreatmentDecision entities record the chosen intervention with clinical justification. MetabolicCollection entities represent individual 24-hour urine collections with completeness validation. PreventionPlan entities document the dietary and pharmacologic prevention strategy tailored to the patient's metabolic profile and stone composition.

## Value Objects

StoneCharacterization captures stone size (mm), location, density (HU), and suspected or confirmed composition. TwentyFourHourUrineResult records all metabolic parameters with abnormality classifications. SupersaturationIndex quantifies the crystallization risk for specific stone types (calcium oxalate, calcium phosphate, uric acid). StoneFreeDetermination records the post-treatment imaging assessment and residual fragment status.

## Domain Events

StoneAnalysisReturned triggers metabolic prevention planning based on confirmed stone composition. MetabolicAbnormalityIdentified initiates dietary counseling and pharmacologic intervention. StoneRecurrenceDetected triggers reassessment of the prevention plan. StoneProcedureRequested sends a request to the Surgical Management Context for ESWL, ureteroscopy, or PCNL. StonePassedSpontaneously records successful conservative management.

## Treatment Selection Algorithm

The context implements evidence-based treatment selection. Ureteral stones less than 10 mm may be managed with medical expulsive therapy and observation. Stones larger than 10 mm or those failing conservative management require intervention. ESWL is preferred for renal stones less than 2 cm with density below 1000 HU. Ureteroscopy is preferred for ureteral stones and smaller renal stones. PCNL is indicated for stones larger than 2 cm, staghorn calculi, or stones in unfavorable anatomy (lower pole with acute infundibulopelvic angle).

## Metabolic Prevention Model

The metabolic prevention model identifies specific metabolic abnormalities and maps them to targeted interventions. Hypercalciuria is addressed with dietary sodium restriction and thiazide diuretics. Hyperoxaluria requires dietary oxalate restriction and calcium supplementation with meals. Hypocitraturia is treated with potassium citrate supplementation. Hyperuricosuria responds to dietary purine restriction and allopurinol. Low urine volume is addressed through increased fluid intake targeting greater than 2.5 liters of urine output daily.

## Integration Points

The Stone Disease Context receives diagnostic imaging results from the Clinical Assessment Context. It sends procedural requests to the Surgical Management Context for ESWL, ureteroscopy, and PCNL, and receives operative outcomes. It integrates with laboratory systems for stone composition analysis and 24-hour urine collection results through an anti-corruption layer. It coordinates with dietary services for nutritional counseling.

## Business Rules

A complete metabolic evaluation requires at least two 24-hour urine collections with adequate creatinine excretion to confirm completeness. Treatment selection must consider stone characteristics, patient anatomy, and patient preferences. Stone-free assessment requires post-treatment imaging at the appropriate interval (typically 4-6 weeks). Metabolic prevention compliance must be monitored with follow-up 24-hour urine collections to verify that interventions are achieving target values.

## Relationship to Other Contexts

The Stone Disease Context is a customer of the Clinical Assessment Context for diagnostic imaging and a customer of the Surgical Management Context for procedural services. It maintains a conformist relationship with AUA and EAU stone management guidelines. It operates independently from the Oncologic Context, though both may evaluate renal imaging findings.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Pearle, M.S. et al. "AUA/Endourology Society Guideline: Medical Management of Kidney Stones." Journal of Urology, 2014.
- Assimos, D. et al. "AUA/Endourology Society Guideline: Surgical Management of Stones." Journal of Urology, 2016.
