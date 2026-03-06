# Stroke Domain - DDD Project Instructions

This domain applies Domain-Driven Design (DDD) to stroke care systems, encompassing acute stroke response, diagnostic imaging and assessment, thrombolytic and interventional treatment, inpatient stroke unit management, rehabilitation and recovery, and secondary prevention and long-term follow-up.

## Bounded Contexts

1. Acute Stroke Response Context - emergency triage, stroke code activation, symptom onset determination, NIHSS scoring, prehospital notification, and door-to-treatment time tracking.
2. Diagnostic Imaging and Assessment Context - CT and CT angiography interpretation, MRI diffusion-weighted imaging, perfusion imaging analysis, large vessel occlusion detection, hemorrhage exclusion, and ASPECTS scoring.
3. Treatment and Intervention Context - intravenous thrombolysis (alteplase/tenecteplase) administration, mechanical thrombectomy decision-making, blood pressure management protocols, and contraindication screening.
4. Stroke Unit Management Context - inpatient bed allocation, nursing assessment protocols, vital sign monitoring schedules, dysphagia screening, DVT prophylaxis, and multidisciplinary team coordination.
5. Rehabilitation and Recovery Context - physical therapy planning, occupational therapy goals, speech-language pathology intervention, cognitive rehabilitation, functional outcome measurement, and discharge planning.
6. Secondary Prevention Context - antiplatelet and anticoagulation therapy selection, statin management, blood pressure optimization, atrial fibrillation screening, carotid stenosis evaluation, and lifestyle modification counseling.

## Domain Principles

- Model the stroke care pathway as a time-critical sequence where elapsed time from symptom onset governs treatment eligibility windows.
- Represent stroke severity using validated scoring systems (NIHSS, mRS, ASPECTS) as first-class value objects in the domain.
- Capture the distinction between ischemic and hemorrhagic stroke as fundamentally different treatment pathways with separate aggregate roots.
- Enforce evidence-based treatment windows (e.g., 4.5-hour thrombolysis window, 24-hour thrombectomy window) as domain invariants.
- Track the complete continuum of care from emergency presentation through long-term secondary prevention as interconnected domain events.

## Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- acute-stroke-response-context
- diagnostic-imaging-assessment-context
- treatment-intervention-context
- stroke-unit-management-context
- rehabilitation-recovery-context
- secondary-prevention-context
- event-driven-architecture
- integration-patterns
- stroke-care-standards
- regulatory-compliance

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Powers, W. J. et al. (2019). Guidelines for the Early Management of Patients with Acute Ischemic Stroke. American Heart Association / American Stroke Association.
- Langhorne, P. et al. (2011). Stroke Unit Care. The Lancet.
