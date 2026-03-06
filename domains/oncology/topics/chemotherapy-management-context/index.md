# Chemotherapy Management Context

## Overview

The Chemotherapy Management Context handles the ordering, preparation, and administration of systemic cancer therapies, including cytotoxic chemotherapy, targeted therapy, immunotherapy, and hormonal therapy. This bounded context translates treatment plan directives into specific drug orders with patient-calculated doses, manages infusion scheduling, monitors treatment toxicity, and applies dose modifications based on clinical response and adverse events. It is one of the most safety-critical contexts in the oncology domain.

## Ubiquitous Language

Within this context, a "regimen" is a defined combination of drugs with specified doses, routes, schedules, and cycle lengths. A "cycle" is one complete iteration of the regimen's drug administration pattern. A "dose" always refers to the patient-specific calculated amount, not the protocol standard dose rate. A "dose modification" is a deliberate adjustment of the calculated dose based on clinical criteria, documented with justification. "Toxicity" is an adverse effect of treatment, graded using the CTCAE scale.

## Aggregate Roots

### Chemotherapy Order

The Chemotherapy Order is the primary aggregate root. It encapsulates the selected regimen, patient-specific dose calculations for each drug, the administration schedule for each cycle, pre-medication and supportive care orders, dose modification history, and cumulative dose tracking. The aggregate enforces critical safety invariants.

Dose limit invariants ensure that no single drug dose exceeds the protocol-defined maximum dose without explicit clinical override with documented justification. Cumulative dose invariants track lifetime cumulative doses for drugs with cumulative toxicity limits (such as doxorubicin's cardiac dose limit). Cycle count invariants ensure the total number of planned cycles does not exceed protocol specifications without documented clinical reasoning.

### Toxicity Assessment

The Toxicity Assessment aggregate records and grades adverse events observed during or after chemotherapy administration. It contains the CTCAE adverse event term, the system organ class, the assigned grade (1 through 5), the attribution to the specific treatment, and the action taken (dose modification, treatment delay, supportive care). The aggregate ensures that each adverse event has a valid CTCAE grade and that the attribution is documented.

## Key Entities

**Chemotherapy Cycle**: An entity representing a single iteration of the regimen. Each cycle has a unique number within the treatment course and tracks the planned drugs with doses, actual administered drugs with doses, administration dates, and observed toxicities. Cycles progress through states: planned, prepared, in-progress, completed, delayed, or cancelled.

**Infusion Session**: An entity representing a single visit during which drugs are administered. Tracks the scheduled time, actual start and end times, the drugs administered during the session, infusion rates, and any infusion reactions. Multiple infusion sessions may occur within a single cycle.

**Dose Modification Record**: An entity documenting each dose adjustment made during treatment. Records the drug affected, the original calculated dose, the modified dose, the modification percentage, the clinical reason (specific toxicity grade), and the authorizing physician.

## Key Value Objects

**Dose Calculation**: The computed dose for a specific drug, including the dose rate from the protocol, the patient's BSA or weight, and the resulting absolute dose.

**Body Surface Area**: The patient's calculated BSA in square meters, derived from height and weight using a specified formula.

**CTCAE Grade**: The severity rating of an adverse event per the CTCAE scale.

**Regimen Protocol**: The canonical regimen definition with drug combination, standard dose rates, routes, cycle length, and planned cycles.

**Cumulative Dose**: The running total of a specific drug administered over the patient's treatment history, used for lifetime dose limit tracking.

## Domain Events

**ChemotherapyOrderCreated**: A new chemotherapy order has been entered based on an approved treatment plan.
**ChemotherapyCycleStarted**: Infusion of the first drug in a cycle has begun.
**ChemotherapyCycleCompleted**: All drugs in a cycle have been administered.
**ChemotherapyCycleDelayed**: A planned cycle has been postponed due to clinical reasons (toxicity, lab values).
**ToxicityRecorded**: An adverse event has been assessed and graded.
**DoseModificationApplied**: A drug dose has been modified from the protocol-defined dose.
**CumulativeDoseLimitApproaching**: A drug's cumulative dose is approaching its lifetime limit.
**TreatmentCourseCompleted**: All planned cycles of the chemotherapy course have been administered.

## Domain Services

**DoseCalculationService**: Computes patient-specific drug doses from protocol dose rates and patient parameters (BSA, weight, organ function). Applies dose caps, rounding rules, and reduction percentages.

**DoseModificationService**: Determines appropriate dose adjustments based on observed toxicity grades and regimen-specific dose modification guidelines.

**RegimenInteractionService**: Checks for clinically significant drug interactions within the regimen and with concurrent medications.

## Integration Points

This context is downstream of the Treatment Planning Context, receiving approved treatment plan directives that specify systemic therapy as a selected modality. The anti-corruption layer translates the treatment plan's modality selection into the chemotherapy context's regimen selection workflow.

This context interfaces with pharmacy systems through an ACL for drug dispensing and inventory verification. It interfaces with laboratory systems for pre-treatment lab value checks (complete blood count, metabolic panel, organ function tests). It publishes toxicity events that the Treatment Planning Context monitors for potential treatment plan revision triggers.

## Safety Considerations

Chemotherapy management is inherently safety-critical. The domain model must enforce dose limits, require clinical justification for modifications, and maintain a complete audit trail of all dose calculations and changes. The aggregate design ensures that dose calculations and dose limits are always evaluated within the same transactional boundary, preventing situations where a dose could be calculated without being checked against limits.

The regimen protocol value object serves as a reference standard against which all orders are validated. Deviations from the protocol, while clinically appropriate in many cases, must be explicitly documented and cannot occur silently.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- National Cancer Institute. Common Terminology Criteria for Adverse Events (CTCAE) v5.0. 2017.
- American Society of Clinical Oncology. Chemotherapy Safety Standards. https://www.asco.org/practice-patients/quality
