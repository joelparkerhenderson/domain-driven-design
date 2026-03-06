# Medication Protocol Context for Mast Cell Activation Syndrome

## Overview

The Medication Protocol Context is a supporting bounded context that governs the pharmacological management of MCAS. This context manages medication selection, dosing, titration, and compounding for multi-drug regimens that typically include H1 antihistamines, H2 antihistamines, mast cell stabilizers, and leukotriene inhibitors. The context addresses the unique challenge that MCAS patients frequently react to inactive ingredients in commercial medication formulations, requiring custom compounding.

The ubiquitous language within this context includes terms such as medication regimen, titration plan, compounding specification, excipient exclusion, tolerability, and pharmacological trigger. These terms have precise meanings within the medication management boundary.

## Key Aggregates

### Medication Regimen

The Medication Regimen is the primary aggregate root. It represents the complete set of medications prescribed for a patient's MCAS management. The regimen contains Medication Entry entities for each prescribed medication, along with their dosages, schedules, and titration plans.

The aggregate enforces several invariants. Medication interactions must be evaluated before adding a new medication. Titration steps must not exceed safe dosage increments for the medication type. Compounding specifications must exclude all excipients identified as triggers in the patient's trigger profile. The regimen maintains a complete history of all changes to support retrospective analysis.

### Compounding Specification

The Compounding Specification is a separate aggregate managing formulation details for custom-compounded medications. It contains the active ingredient, dosage form, strength, and an explicit exclusion list of dyes, fillers, and excipients. The aggregate ensures that compounding instructions are complete and that the exclusion list reflects the patient's current trigger profile.

## Value Objects

The Dosage value object represents the prescribed amount for a single administration, including the numeric value, unit of measurement, and route of administration. The Titration Step value object captures a single adjustment in a titration schedule, including the target dosage, the duration at that level, and monitoring requirements.

The Excipient Exclusion value object identifies a specific inactive ingredient excluded from compounded formulations, with the exclusion reason and documentation date. The Tolerability Assessment value object records whether a patient can take a specific medication formulation without adverse reaction.

## Medication Categories

### H1 Antihistamines

H1 antihistamines are typically the first-line medications in MCAS management. Second-generation H1 antihistamines such as cetirizine, loratadine, and fexofenadine are preferred for their lower sedation profiles. Some patients require higher-than-standard doses, and the context model supports supratherapeutic dosing protocols with appropriate clinical justification and monitoring requirements.

### H2 Antihistamines

H2 antihistamines such as famotidine address gastrointestinal symptoms including acid reflux, abdominal pain, and nausea. They complement H1 antihistamines by blocking a different histamine receptor subtype. The context tracks the rationale for H2 antihistamine selection and monitors for known side effects.

### Mast Cell Stabilizers

Mast cell stabilizers such as cromolyn sodium and ketotifen prevent mast cell degranulation rather than blocking the effects of released mediators. Cromolyn sodium is used primarily for gastrointestinal symptoms, while ketotifen provides both mast cell stabilization and H1 antihistamine effects. The context manages the specific titration protocols for these medications, which often require gradual dose escalation.

### Leukotriene Inhibitors

Leukotriene inhibitors such as montelukast and zileuton block the effects of leukotrienes, which are potent inflammatory mediators released during mast cell activation. The context tracks the specific leukotriene pathway targeted and monitors for medication-specific adverse effects.

### Additional Medications

The context also manages adjunctive medications that may be prescribed for MCAS, including aspirin for prostaglandin-mediated symptoms, vitamin C as a natural antihistamine adjunct, quercetin and other flavonoids, benzodiazepines for acute flare management, and epinephrine for anaphylaxis preparedness.

## Domain Events

MedicationStarted is published when a new medication is added to the regimen. MedicationTitrated is published when a dosage is adjusted. MedicationDiscontinued is published when a medication is removed. All of these events are consumed by the Outcomes Measurement Context for effectiveness analysis and by the Specialist Coordination Context for care plan updates.

## Domain Services

The InteractionCheckService evaluates proposed medication changes against the current regimen for pharmacological interactions and against the patient's trigger profile for tolerability concerns. The TitrationPlanGenerationService creates individualized titration schedules based on medication type, patient sensitivity, and clinical guidelines.

## Compounding Considerations

Compounding is a distinguishing feature of MCAS medication management. Many commercial medication formulations contain inactive ingredients such as dyes, fillers, binders, coatings, and flavorings that may trigger mast cell activation. The context model tracks which excipients are problematic for each patient and ensures that compounding specifications exclude them.

The context manages relationships with compounding pharmacies, including their capabilities, excipient inventories, and quality assurance processes. Compounding specifications include detailed instructions for the pharmacist, including the active ingredient source, the dosage form, and any special handling requirements.

## Titration Protocols

MCAS patients often require slower titration than standard protocols due to heightened sensitivity to medication changes. The context supports individualized titration schedules that may start at fractions of standard doses and progress in small increments over extended periods. Each titration step includes monitoring criteria that must be met before advancing to the next step.

## Integration Points

The Medication Protocol Context consumes diagnostic events from the Diagnostic Assessment Context to inform initial medication selection. It consumes trigger data from the Trigger Management Context to ensure medications do not contain known triggers. It publishes medication events that the Outcomes Measurement Context uses for effectiveness analysis.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Afrin, L.B. et al. "Diagnosis of Mast Cell Activation Syndrome: A Global Consensus-2." Diagnosis, 2020.
- Molderings, G.J. et al. "Pharmacological Treatment Options for Mast Cell Activation Disease." Naunyn-Schmiedeberg's Archives of Pharmacology, 2016.
