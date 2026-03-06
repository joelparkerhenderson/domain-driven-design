# Diagnostic Assessment Context for Mast Cell Activation Syndrome

## Overview

The Diagnostic Assessment Context is the core bounded context in the MCAS domain. It manages the complete diagnostic journey from initial clinical suspicion through confirmed diagnosis or exclusion. This context owns laboratory test ordering, specimen tracking, result interpretation, and consensus criteria evaluation. As the core subdomain, it receives the most rigorous domain modeling effort because diagnostic accuracy is the foundation upon which all MCAS management depends.

The context operates with a rich ubiquitous language drawn from clinical immunology and laboratory medicine. Terms such as tryptase level, histamine metabolite, prostaglandin D2, mediator elevation, and consensus criteria carry precise meanings within this context that may not be shared by other contexts.

## Key Aggregates

### Diagnostic Workup

The Diagnostic Workup is the primary aggregate root. It represents a complete evaluation episode and contains Mediator Panels, individual Mediator Levels, and Criteria Evaluations. The workup progresses through defined states: initiated, in-progress, pending-interpretation, and concluded. All modifications to diagnostic data within the workup occur through the aggregate root to maintain consistency.

The workup enforces several invariants. A diagnosis cannot be confirmed until all three consensus criteria elements have been evaluated. Mediator levels cannot be interpreted without an established baseline. Criteria evaluation cannot proceed until all required laboratory results have been received.

### Bone Marrow Assessment

The Bone Marrow Assessment is a separate aggregate because it has an independent lifecycle. Bone marrow biopsies are not required for all patients and may take weeks to complete due to pathology processing. This aggregate tracks the procedure from scheduling through pathological interpretation and determines whether clonal mast cell disease markers are present.

## Value Objects

The Tryptase Level value object captures serum tryptase measurements with the measured value, collection timestamp, and specimen type. It includes behavior to calculate elevation above baseline using the formula: clinically significant if acute value exceeds (1.2 times baseline) plus 2 ng/mL.

The Histamine Metabolite Level value object represents urine N-methylhistamine measurements. The Prostaglandin D2 Level value object captures urine 11-beta-prostaglandin F2-alpha measurements. Both include reference range comparison behavior.

The Consensus Criteria Result value object encapsulates the three-element evaluation: symptomatic episodes, mediator elevation documentation, and treatment response demonstration.

## Domain Events

The Diagnostic Assessment Context publishes several critical events. DiagnosticWorkupInitiated signals the start of evaluation. MediatorLevelRecorded communicates new laboratory results. DiagnosisConfirmed is the pivotal event that triggers downstream actions across the system. DiagnosisRuledOut communicates that MCAS criteria were not met.

These events are consumed by multiple downstream contexts. A confirmed diagnosis triggers trigger profile creation, medication regimen initiation, care team assembly, and outcomes tracking setup.

## Domain Services

The ConsensusCriteriaEvaluationService implements the clinical reasoning process for evaluating diagnostic data against consensus criteria. The BaselineCalculationService computes patient-specific baseline mediator levels from historical measurements.

## Diagnostic Criteria

The context supports multiple diagnostic criteria frameworks. The primary framework requires three elements: clinical symptoms consistent with mast cell mediator release affecting two or more organ systems, laboratory documentation of an increase in a validated mast cell mediator during a symptomatic episode compared to the patient's baseline, and clinical improvement with medications that target mast cell mediators or their effects.

Validated mediator measurements include serum tryptase, urine N-methylhistamine, urine prostaglandin D2 metabolite, and urine leukotriene E4. The context model accommodates evolving criteria as research advances understanding of MCAS.

## Integration with External Systems

The Diagnostic Assessment Context integrates with laboratory information systems through an anti-corruption layer. External laboratory results in LOINC-coded formats are translated into the domain's Mediator Level value objects. The ACL handles unit conversion, reference range mapping, and result status translation.

The context also integrates with electronic health records for patient demographic information and clinical history. An ACL translates HL7 FHIR Patient resources into the domain's patient model.

## Clinical Workflow

The diagnostic workflow begins with a clinician ordering a mediator panel during a symptomatic episode and, separately, during an asymptomatic period for baseline comparison. Results flow through the anti-corruption layer into the Diagnostic Workup aggregate. The ConsensusCriteriaEvaluationService assesses results against criteria. If additional testing (such as bone marrow biopsy) is needed, a Bone Marrow Assessment aggregate is created.

The workup concludes when sufficient evidence exists to confirm or rule out MCAS. The conclusion is published as a domain event that drives downstream context activation.

## Differential Diagnosis Considerations

The context model accounts for conditions that must be differentiated from MCAS, including systemic mastocytosis, hereditary alpha-tryptasemia, carcinoid syndrome, pheochromocytoma, and idiopathic anaphylaxis. The Diagnostic Workup aggregate may include exclusion test results that rule out these alternative diagnoses.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Valent, P. et al. "Proposed Diagnostic Algorithm for Patients with Suspected Mast Cell Activation Syndrome." Journal of Allergy and Clinical Immunology: In Practice, 2019.
- Molderings, G.J. et al. "Mast Cell Activation Disease: A Concise Practical Guide for Diagnostic Workup and Therapeutic Options." Journal of Hematology & Oncology, 2011.
