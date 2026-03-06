# Neurorehabilitation Context - Neurology Domain

## Overview

The Neurorehabilitation Context is a supporting bounded context that manages recovery and rehabilitation programs following neurological injury or disease. This context tracks functional outcomes using standardized assessment instruments, manages therapy protocols across multiple disciplines, and coordinates the transition from acute care through rehabilitation to community reintegration.

Neurorehabilitation operates on a fundamentally different temporal model compared to other neurology bounded contexts. While Clinical Assessment captures point-in-time evaluations and Neurodegenerative Disease tracks progressive decline, Neurorehabilitation models recovery trajectories with expected improvements over time.

## Scope and Responsibilities

### Stroke Recovery

**Acute Phase (0-7 days)**
- Stroke severity assessment using NIHSS (National Institutes of Health Stroke Scale).
- Early mobilization protocols.
- Dysphagia screening and swallowing assessment.
- Initial rehabilitation needs assessment.
- Transfer planning to rehabilitation facility or program.

**Subacute Phase (1 week - 3 months)**
- Intensive inpatient or outpatient rehabilitation.
- Motor recovery tracking: upper extremity and lower extremity function.
- Aphasia evaluation and speech-language therapy initiation.
- ADL (Activities of Daily Living) retraining.
- Spasticity management: stretching programs, positioning, pharmacotherapy (baclofen, tizanidine, botulinum toxin).

**Chronic Phase (3+ months)**
- Community-based rehabilitation and maintenance therapy.
- Long-term functional outcome tracking.
- Secondary stroke prevention adherence monitoring.
- Mood and emotional adjustment assessment (post-stroke depression screening).
- Return to work and driving evaluation.

### Traumatic Brain Injury (TBI) Rehabilitation

**Severity Classification**
- Mild TBI/concussion: GCS 13-15, brief or no loss of consciousness.
- Moderate TBI: GCS 9-12.
- Severe TBI: GCS 3-8.

**Post-Traumatic Recovery Tracking**
- Rancho Los Amigos Levels of Cognitive Functioning Scale (Levels I-VIII).
- Post-traumatic amnesia duration monitoring.
- Behavioral and emotional regulation assessment.
- Cognitive rehabilitation: attention, memory, executive function, processing speed.
- Community reintegration planning.

**Concussion Management**
- Symptom tracking: headache, dizziness, cognitive fog, light/noise sensitivity.
- Graduated return-to-activity protocol.
- Return-to-sport decision making.
- Post-concussion syndrome monitoring.

### Functional Outcome Measurement

**Functional Independence Measure (FIM)**
- 18-item scale measuring motor (13 items) and cognitive (5 items) function.
- Each item scored 1 (total assistance) to 7 (complete independence).
- Total score range: 18 (fully dependent) to 126 (fully independent).
- Subscale tracking: self-care, sphincter control, transfers, locomotion, communication, social cognition.
- FIM gain (admission to discharge) as a primary rehabilitation outcome metric.

**Modified Rankin Scale (mRS)**
- 7-point scale (0-6) for post-stroke disability.
- Grade 0: no symptoms. Grade 1: no significant disability. Grade 2: slight disability. Grade 3: moderate disability. Grade 4: moderately severe disability. Grade 5: severe disability. Grade 6: death.
- Used as primary endpoint in stroke clinical trials.

**Barthel Index**
- 10-item scale for ADL independence.
- Total score 0-100 in increments of 5.
- Items: feeding, bathing, grooming, dressing, bowels, bladder, toilet use, transfers, mobility, stairs.

**NIH Stroke Scale (NIHSS)**
- 15-item acute stroke severity scale, score range 0-42.
- Used at baseline and serially to track early neurological change.

### Cognitive Rehabilitation

- Attention training: sustained, selective, divided, alternating attention exercises.
- Memory rehabilitation: compensatory strategies, errorless learning, spaced retrieval.
- Executive function training: planning, problem-solving, cognitive flexibility, self-monitoring.
- Language rehabilitation: naming therapy, constraint-induced language therapy, communication strategies.
- Computer-assisted cognitive rehabilitation programs.

### Adaptive Technology

- Assistive communication devices: augmentative and alternative communication (AAC) for aphasia and dysarthria.
- Mobility aids: canes, walkers, wheelchairs, power mobility devices.
- Environmental modifications: home safety assessment, accessibility modifications.
- Computer access technology: adaptive keyboards, eye-tracking systems, switch access.
- Orthotic devices: ankle-foot orthosis (AFO), wrist-hand orthosis.

## Aggregate Design

**RehabilitationEpisode** is the primary aggregate root, tracking a single episode of rehabilitation care. It enforces the invariant that a discharge assessment must be completed before the episode can be closed.

**FunctionalOutcome** is a separate aggregate root recording standardized functional assessments with self-validating scores.

## Domain Events

- RehabilitationEpisodeStarted: initiates therapy scheduling and goal setting.
- FunctionalOutcomeAssessed: triggers progress evaluation and goal adjustment.
- RehabilitationGoalAchieved: triggers care plan revision or discharge readiness evaluation.
- DischargeRecommended: initiates community transition planning.

## Integration Points

- Receives baseline assessment data from Clinical Assessment Context (downstream).
- Receives disease progression events from Neurodegenerative Disease Context (partnership).
- Receives diagnostic conclusions from Neuromuscular Disorders Context.
- Publishes functional outcome data for longitudinal tracking.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Cifu, David X. *Braddom's Physical Medicine and Rehabilitation*. 6th Edition. Elsevier, 2020.
- Winstein, Carolee J., et al. "Guidelines for Adult Stroke Rehabilitation and Recovery." *Stroke*, 2016.
