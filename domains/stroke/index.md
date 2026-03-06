# Stroke Domain - Domain-Driven Design

## Introduction

The stroke domain applies Domain-Driven Design to the clinical management of cerebrovascular accidents (strokes), encompassing the entire continuum of care from emergency presentation through acute treatment, inpatient management, rehabilitation, and long-term secondary prevention. Stroke is a time-critical medical emergency where the elapsed time from symptom onset to treatment directly determines patient outcomes, famously summarized by the clinical axiom "time is brain." The domain must model the distinction between ischemic stroke (caused by arterial occlusion) and hemorrhagic stroke (caused by vessel rupture), as these represent fundamentally different pathophysiological processes requiring divergent treatment approaches.

The domain is organized into six bounded contexts that reflect the natural phases of stroke care delivery: acute stroke response, diagnostic imaging and assessment, treatment and intervention, stroke unit management, rehabilitation and recovery, and secondary prevention. Each bounded context maintains its own internal consistency while communicating with other contexts through time-stamped domain events that preserve the critical temporal relationships governing treatment eligibility, quality metric calculation, and care transitions across the stroke pathway.

## Bounded Contexts

1. **Acute Stroke Response Context** - Manages the initial emergency response to a suspected stroke, including prehospital notification, emergency department triage, stroke code activation, symptom onset time determination, and rapid neurological assessment using the NIH Stroke Scale (NIHSS). Every action in this context is timestamped to support door-to-treatment quality metrics.

2. **Diagnostic Imaging and Assessment Context** - Handles the acquisition and interpretation of neuroimaging studies including non-contrast CT, CT angiography, CT perfusion, and MRI diffusion-weighted imaging. This context determines stroke type (ischemic vs. hemorrhagic), identifies large vessel occlusions, calculates ASPECTS scores, and assesses salvageable brain tissue through perfusion mismatch analysis.

3. **Treatment and Intervention Context** - Manages the administration of acute stroke therapies including intravenous thrombolysis (alteplase or tenecteplase) and mechanical thrombectomy. This context enforces treatment eligibility windows, screens for contraindications, manages blood pressure protocols, and records procedural details including reperfusion outcomes.

4. **Stroke Unit Management Context** - Coordinates inpatient care on the dedicated stroke unit, including nursing assessment protocols, continuous vital sign monitoring, dysphagia screening before oral intake, DVT prophylaxis, and multidisciplinary team rounds. This context manages bed allocation and tracks neurological status changes.

5. **Rehabilitation and Recovery Context** - Plans and tracks post-stroke rehabilitation across physical therapy, occupational therapy, speech-language pathology, and cognitive rehabilitation disciplines. Functional outcomes are measured using validated scales (modified Rankin Scale, Barthel Index) and inform discharge planning and community reintegration goals.

6. **Secondary Prevention Context** - Manages the long-term strategy to prevent recurrent stroke, including antiplatelet or anticoagulation therapy selection, statin management, blood pressure optimization, atrial fibrillation detection and treatment, carotid stenosis evaluation, and lifestyle modification counseling. This context coordinates ongoing outpatient follow-up visits.

## Strategic Design

- The stroke domain is classified as a core subdomain where the time-critical acute care pathway serves as the primary value driver, with treatment eligibility determined by elapsed time from symptom onset.
- Context mapping uses an upstream-downstream relationship where the Diagnostic Imaging Context produces findings that the Treatment and Intervention Context consumes to determine treatment eligibility.
- Anti-corruption layers translate data from external PACS (picture archiving and communication systems), electronic health records, and telestroke consultation platforms into the domain's ubiquitous language.

## Tactical Design

- **Stroke Encounter Aggregate** - The root aggregate representing a single stroke event for a patient, maintaining the timeline from symptom onset through discharge, the stroke classification, and all treatment decisions.
- **NIHSS Score Value Object** - An immutable neurological severity assessment containing individual item scores and a total score, taken at specific time points to track clinical trajectory.
- **Treatment Window Value Object** - Encapsulates the time-bound eligibility period for a specific intervention (e.g., 4.5 hours for IV thrombolysis, 24 hours for mechanical thrombectomy), calculated from symptom onset or last known well time.
- **Stroke Code Activated Domain Event** - Raised when a suspected stroke is identified, triggering parallel workflows across emergency response, imaging, and neurology consultation contexts.
- **Imaging Study Entity** - A diagnostic neuroimaging examination with its modality, acquisition time, findings, and interpretation, tracked as part of the diagnostic assessment workflow.
- **Treatment Eligibility Assessor Domain Service** - Evaluates whether a patient qualifies for specific acute interventions based on elapsed time, imaging findings, clinical severity, and contraindication screening.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Powers, W. J. et al. (2019). *Guidelines for the Early Management of Patients with Acute Ischemic Stroke*. American Heart Association / American Stroke Association.
- Langhorne, P. et al. (2011). *Stroke Unit Care*. The Lancet.
