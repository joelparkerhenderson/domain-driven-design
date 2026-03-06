# Medication Management Context: Attention-Deficit Domain

The Medication Management Context handles pharmacological treatment for ADHD, encompassing stimulant and non-stimulant medication protocols, dosage titration schedules, side effect monitoring, medication adherence tracking, and prescriber communication.

## Context Purpose

Medication is a cornerstone of ADHD management for most patients aged six and older. This bounded context manages the complexities of pharmacological treatment, including medication selection based on patient characteristics and comorbidities, systematic dosage optimization through titration, ongoing side effect surveillance, and medication adherence monitoring. It operates downstream of the Clinical Assessment Context, receiving diagnostic confirmation that initiates pharmacological treatment, and publishes medication change events consumed by the Behavioral Monitoring and Outcomes Tracking contexts.

## Aggregate Root: Medication Regimen

The Medication Regimen aggregate represents the current and historical pharmacological treatment for a patient. It encapsulates medication trials, dosage adjustments, side effect reports, and adherence records within a single transactional boundary.

Key invariants enforced by this aggregate:

- Active medications must not have known contraindications with each other or with medications prescribed for comorbid conditions.
- Dosage adjustments must fall within the FDA-approved therapeutic range for the specific medication and patient age group.
- Titration steps must follow the prescribed minimum interval between dosage changes (typically one to two weeks for stimulants).
- Controlled substance prescriptions must comply with DEA scheduling requirements and state prescribing regulations.
- Side effect reports exceeding a defined severity threshold must trigger a prescriber review event.

## Core Domain Concepts

### Stimulant Medications

First-line pharmacological treatment for ADHD, with two primary classes:

- **Methylphenidate-Based Medications**: Available in immediate-release (Ritalin, Methylin), extended-release (Concerta, Ritalin LA, Metadate CD, Quillivant XR), and long-acting (Daytrana patch) formulations. Mechanism: blocks dopamine and norepinephrine reuptake. Typical dosage range: 5-60 mg/day depending on formulation and patient age.
- **Amphetamine-Based Medications**: Available in immediate-release (Adderall, Dexedrine), extended-release (Adderall XR, Vyvanse, Mydayis), and mixed salt formulations. Mechanism: increases release and blocks reuptake of dopamine and norepinephrine. Typical dosage range: 5-40 mg/day depending on formulation.
- **Formulation Selection Considerations**: Duration of coverage needed (school day only vs. full day), swallowing ability (liquid, chewable, sprinkle capsule options), abuse potential concerns (prodrug formulations like Vyvanse have lower abuse potential), and insurance formulary coverage.

### Non-Stimulant Medications

Alternative pharmacological options when stimulants are ineffective, poorly tolerated, or contraindicated:

- **Atomoxetine (Strattera)**: Selective norepinephrine reuptake inhibitor. Provides 24-hour symptom coverage. Typical dosage: 0.5-1.4 mg/kg/day. Titrated over 4-6 weeks. Particularly useful when anxiety comorbidity is present or when stimulant diversion is a concern.
- **Guanfacine Extended-Release (Intuniv)**: Alpha-2A adrenergic agonist. Effective for hyperactivity and impulsivity symptoms. Typical dosage: 1-4 mg/day for children. Can be used as monotherapy or adjunct to stimulants.
- **Clonidine Extended-Release (Kapvay)**: Alpha-2 adrenergic agonist. Similar mechanism to guanfacine but less selective. Typical dosage: 0.1-0.4 mg/day. Often used for ADHD with comorbid tic disorders or sleep disturbance.
- **Viloxazine (Qelbree)**: Selective norepinephrine reuptake inhibitor approved for ADHD in children 6-17 and adults. Typical dosage: 100-400 mg/day for children; 200-600 mg/day for adults.

### Titration Protocol

The systematic dosage optimization process:

1. **Initiation**: Start at the lowest recommended dose based on medication type, patient age, and weight.
2. **Monitoring Period**: Observe for one to two weeks at each dosage level, collecting symptom response and side effect data.
3. **Dose Adjustment**: Increase dosage by the minimum increment if symptom response is insufficient and side effects are tolerable.
4. **Optimal Dose Identification**: The dose at which maximum symptom improvement is achieved with acceptable side effects.
5. **Maintenance**: Continue at the optimal dose with periodic reassessment (typically every 3-6 months).
6. **Trial Conclusion**: If a medication fails to achieve adequate response at the maximum dose or produces intolerable side effects, the trial is concluded and an alternative medication is considered.

### Side Effect Monitoring

Systematic surveillance of adverse effects:

- **Common Stimulant Side Effects**: Appetite suppression, weight loss, insomnia, headache, stomachache, irritability, mood lability. Monitored through patient/parent report and clinician assessment.
- **Cardiovascular Monitoring**: Heart rate and blood pressure measurement at baseline and each dose adjustment. Screening for cardiac risk factors before stimulant initiation.
- **Growth Monitoring**: Height and weight tracking at regular intervals to detect growth suppression, a known long-term concern with stimulant treatment.
- **Psychiatric Side Effects**: Monitoring for new or worsening anxiety, depression, psychotic symptoms, or tic emergence/exacerbation.
- **Non-Stimulant Specific Monitoring**: Liver function monitoring for atomoxetine, sedation and hypotension monitoring for alpha-2 agonists.

### Medication Adherence

Tracking and supporting consistent medication use:

- **Adherence Recording**: Documentation of medication taking, missed doses, and reasons for non-adherence.
- **Adherence Barriers**: Identification of common barriers including forgetting, side effects, stigma concerns, cost, and difficulty with medication administration schedule.
- **Medication Holidays**: Planned periods without medication (typically weekends or school breaks) to assess ongoing medication need and manage side effects such as appetite suppression.

## Domain Events Produced

- **MedicationTrialInitiated**: Triggers intensified behavioral monitoring.
- **DosageAdjusted**: Triggers behavioral response and side effect observation.
- **SideEffectReported**: Triggers clinical review if severity threshold exceeded.
- **MedicationDiscontinued**: Triggers treatment plan review.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Wolraich, M. L., et al. (2019). Clinical Practice Guideline for the Diagnosis, Evaluation, and Treatment of ADHD in Children and Adolescents. *Pediatrics*, 144(4).
- Cortese, S., et al. (2018). Comparative efficacy and tolerability of medications for ADHD in children, adolescents, and adults: a systematic review and network meta-analysis. *Lancet Psychiatry*, 5(9), 727-738.
