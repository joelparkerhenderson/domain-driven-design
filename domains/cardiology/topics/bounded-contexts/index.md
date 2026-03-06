# Bounded Contexts in Cardiology

## Overview

Bounded contexts are the central pattern in Domain-Driven Design for managing complexity. In the cardiology domain, each bounded context defines a logical boundary within which a specific model of cardiology practice is internally consistent. Terms, rules, and relationships that hold within one bounded context may have different meanings or not exist at all in another. This reflects the reality of cardiology, where an echocardiographer, an interventional cardiologist, and an electrophysiologist each operate with distinct but overlapping vocabularies and workflows.

## The Six Cardiology Bounded Contexts

### Clinical Assessment Context

This context encompasses the initial and ongoing evaluation of cardiac patients. It models patient history intake, cardiovascular physical examination findings, risk score calculations (Framingham, ASCVD, HEART score), cardiac biomarker interpretation (troponin, BNP, NT-proBNP), and stress testing protocols (exercise, pharmacological, nuclear). Within this boundary, a "finding" refers to a clinical observation from history or physical examination, and "risk" is quantified through validated scoring instruments.

### Diagnostic Imaging Context

This context manages the full lifecycle of cardiac imaging studies. It models study orders, image acquisition protocols, structured measurement sets, and finalized reports. Echocardiography, cardiac CT (including calcium scoring and coronary CTA), cardiac MRI, and nuclear cardiology each have distinct measurement vocabularies. Within this boundary, "study" refers to a complete imaging examination, and "measurement" refers to a quantified imaging parameter with reference ranges.

### Interventional Procedures Context

This context handles catheter-based and surgical cardiac interventions. It models pre-procedural planning (including SYNTAX score calculation and Heart Team discussions), procedural execution (catheterization, PCI, stenting, TAVR), device selection and implantation records, and post-procedural complication tracking. Within this boundary, "lesion" refers to a specific coronary artery stenosis, and "intervention" refers to a therapeutic action performed during catheterization.

### Electrophysiology Context

This context addresses cardiac rhythm disorders and their treatment. It models arrhythmia classification, electrophysiology study recordings, ablation procedure mapping and lesion sets, and cardiac implantable electronic device (CIED) management including pacemakers, ICDs, and cardiac resynchronization therapy (CRT) devices. Within this boundary, "signal" refers to intracardiac electrical recordings, and "device" refers specifically to an implanted cardiac rhythm management device.

### Heart Failure Management Context

This context manages the longitudinal care of heart failure patients. It models heart failure phenotype classification (HFrEF, HFmrEF, HFpEF), NYHA functional class assessment, ACC/AHA staging, GDMT titration protocols, volume status monitoring, and escalation pathways to mechanical circulatory support or transplant evaluation. Within this boundary, "therapy" refers to guideline-directed pharmacological treatment, and "stage" refers to the ACC/AHA heart failure progression classification.

### Cardiac Rehabilitation Context

This context supports structured cardiac recovery programs. It models exercise prescription (mode, intensity, duration, frequency), metabolic equivalent (MET) capacity assessment, cardiovascular risk factor modification targets (lipids, blood pressure, glucose, smoking cessation, weight management), psychosocial screening, and outcomes tracking. Within this boundary, "program" refers to a multi-week supervised rehabilitation plan, and "goal" refers to a measurable patient outcome target.

## Boundary Enforcement

Each bounded context enforces its boundary by maintaining its own domain model, independent of other contexts. When concepts must cross boundaries, they are translated through explicit integration mechanisms such as anti-corruption layers, published languages, or shared kernels. For example, when the Clinical Assessment Context determines that a patient needs an imaging study, it publishes an event that the Diagnostic Imaging Context translates into its own study order model.

## Linguistic Boundaries

The same term may carry different meanings across contexts. "Report" in the Diagnostic Imaging Context refers to a structured imaging interpretation document, while in the Clinical Assessment Context it may refer to a clinical summary. "Device" in the Electrophysiology Context refers to an implanted CIED, while in the Interventional Procedures Context it may refer to a stent or valve prosthesis. These linguistic differences are natural and expected; bounded contexts make them explicit rather than forcing artificial unification.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on maintaining model integrity.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 6 on bounded context boundaries.
- ACC/AHA 2021 Chest Pain Guideline for delineation of clinical assessment pathways.
