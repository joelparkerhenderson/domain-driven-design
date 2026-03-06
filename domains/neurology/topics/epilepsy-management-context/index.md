# Epilepsy Management Context - Neurology Domain

## Overview

The Epilepsy Management Context is a supporting bounded context dedicated to the specialized management of epilepsy, from initial seizure presentation through long-term treatment optimization and surgical evaluation. This context uses the International League Against Epilepsy (ILAE) classification system as its foundational framework for seizure and epilepsy categorization.

Epilepsy affects approximately 1% of the population and requires specialized management that differs fundamentally from other neurological conditions. The iterative cycle of seizure recording, medication adjustment, and response evaluation creates a distinct domain model that warrants its own bounded context.

## Scope and Responsibilities

### Seizure Classification

The ILAE 2017 classification system provides the taxonomy for seizure events within this context:

**By Onset Type**
- Focal onset: seizures beginning in one hemisphere, further classified by awareness (aware vs. impaired awareness) and motor involvement (motor onset vs. non-motor onset).
- Generalized onset: seizures engaging bilateral networks from onset, classified as motor (tonic-clonic, tonic, clonic, myoclonic, atonic) or non-motor (absence).
- Unknown onset: seizures where onset cannot be determined.

**Seizure Documentation**
- Semiological description: aura, motor features, automatisms, lateralizing signs.
- Duration and temporal pattern.
- Precipitating factors: sleep deprivation, alcohol, photic stimulation, stress, medication non-compliance.
- Postictal state: confusion, Todd's paralysis, language deficit, headache.
- Witness observations and video documentation.

### Antiepileptic Drug (AED) Management

**Drug Selection**
- First-line monotherapy by seizure type: levetiracetam, lamotrigine, carbamazepine, valproate, oxcarbazepine.
- Adjunctive therapy options: lacosamide, brivaracetam, perampanel, clobazam, zonisamide, topiramate.
- Special considerations: valproate contraindicated in women of childbearing potential, carbamazepine-HLA-B*1502 screening in certain populations.

**Titration Management**
- Starting dose, titration schedule, and target dose tracking.
- Side effect monitoring: cognitive effects, mood changes, dermatologic reactions, hepatic function.
- Therapeutic drug monitoring: serum levels for phenytoin, carbamazepine, valproate, phenobarbital.

**Drug Resistance Evaluation**
- ILAE definition: failure of adequate trials of two tolerated and appropriately chosen AED schedules.
- Documentation of each AED trial: medication, maximum tolerated dose, duration, reason for failure (lack of efficacy vs. intolerable side effects).

### Epilepsy Monitoring Unit (EMU)

**Admission Management**
- Indication documentation: seizure classification, pre-surgical evaluation, non-epileptic event differentiation.
- Continuous video-EEG monitoring setup and management.
- Medication taper protocols for seizure provocation.
- Safety protocols: seizure precautions, IV access, rescue medication availability.

**Seizure Capture and Analysis**
- Electroclinical correlation: matching clinical semiology with EEG ictal patterns.
- Seizure onset zone localization based on EEG electrode coverage.
- Interictal epileptiform discharge mapping.
- Event classification: epileptic seizure vs. psychogenic non-epileptic event (PNEE).

### Surgical Evaluation

**Phase I Evaluation**
- Non-invasive data concordance: seizure semiology, scalp EEG, MRI (epilepsy protocol), PET, neuropsychological testing.
- Multidisciplinary epilepsy surgery conference review.
- Determination of surgical candidacy.

**Phase II Evaluation**
- Invasive monitoring: stereo-EEG (sEEG), subdural grid/strip electrodes.
- Eloquent cortex mapping: motor, language, memory.
- Surgical planning: resection boundaries, expected outcomes, surgical risks.

**Surgical Procedures**
- Anterior temporal lobectomy for mesial temporal lobe epilepsy.
- Lesionectomy for lesional epilepsy.
- Corpus callosotomy for drop attacks.
- Laser interstitial thermal therapy (LITT) for focal lesions.

### Neuromodulation

**Vagus Nerve Stimulation (VNS)**
- Device implantation tracking.
- Programming parameters: output current, signal frequency, pulse width, on/off time.
- Response monitoring: seizure frequency reduction, magnet swipe usage.
- Battery life monitoring and replacement planning.

**Responsive Neurostimulation (RNS)**
- Implantation and lead placement.
- Detection algorithm configuration.
- Stimulation parameter optimization.
- Chronic ECoG data review.

## Aggregate Design

**SeizureEpisode** is an aggregate root capturing individual seizure events with ILAE classification. The invariant ensures classification follows the ILAE hierarchy.

**AEDRegimen** is an aggregate root managing the complete antiepileptic drug regimen, enforcing dosage limits and tracking titration schedules.

## Domain Events

- SeizureRecorded: triggers seizure frequency recalculation and treatment review.
- AEDRegimenAdjusted: notifies pharmacy integration and therapeutic drug monitoring.
- SurgicalCandidacyDetermined: may trigger additional imaging orders.
- DrugResistanceDetermined: escalates to surgical evaluation pathway.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Fisher, Robert S., et al. "Operational Classification of Seizure Types by the ILAE." *Epilepsia*, 2017.
- Kwan, Patrick, et al. "Definition of Drug Resistant Epilepsy." *Epilepsia*, 2010.
- Engel, Jerome Jr. "What Can We Do for People with Drug-Resistant Epilepsy?" *Neurology*, 2016.
