# Bounded Contexts in the Urology Domain

## Overview

A bounded context is a logical boundary within which a specific domain model and its terminology are consistent and unambiguous. In the urology domain, six bounded contexts partition the clinical landscape into coherent areas, each with its own model, rules, and ubiquitous language. These boundaries reflect the natural divisions in urological practice where terminology, clinical reasoning, and workflow patterns differ.

## The Six Urology Bounded Contexts

### 1. Clinical Assessment Context

This context encompasses the diagnostic phase of urological care. It owns the patient's urological history, physical examination findings, symptom scores (IPSS, AUA Symptom Index, SHIM), urodynamic study results, cystoscopy findings, and imaging interpretations. The model within this context focuses on transforming raw clinical data into structured diagnostic profiles that downstream contexts consume.

### 2. Surgical Management Context

This context manages all procedural interventions across urological subspecialties. It owns surgical planning, operative protocols, intraoperative decision trees, and post-operative recovery pathways. The model encompasses robotic-assisted procedures, laparoscopic techniques, open surgical approaches, and endoscopic interventions. Surgical Management maintains its own vocabulary around instrumentation, operative steps, and complication classification (Clavien-Dindo).

### 3. Oncologic Urology Context

This context governs the detection, staging, treatment selection, and surveillance of urological malignancies. It owns cancer-specific models including TNM staging, Gleason grading (ISUP Grade Groups), PSA kinetics, risk stratification (D'Amico, CAPRA), and surveillance protocols. The Oncologic context has its own temporal model tracking disease progression over years or decades.

### 4. Stone Disease Context

This context addresses nephrolithiasis from initial presentation through long-term metabolic prevention. It owns stone composition models, metabolic risk factor profiles, procedural selection algorithms (based on stone size, location, and composition), and recurrence prediction models. The language within this context revolves around crystallography, urinary supersaturation, and lithotripsy physics.

### 5. Incontinence Management Context

This context covers the evaluation and treatment of urinary incontinence. It owns incontinence classification models, pelvic floor assessment protocols, bladder diary data, treatment escalation pathways, and surgical outcome measures (pad counts, quality of life scores). The model distinguishes between neurogenic and non-neurogenic causes and tracks treatment response over time.

### 6. Male Reproductive Health Context

This context manages male fertility, sexual function, and hormonal health. It owns semen analysis interpretation models, hormonal profiles, erectile function assessment (IIEF scores), surgical fertility intervention protocols, and testosterone replacement monitoring. The language includes reproductive physiology terms distinct from other urology contexts.

## Why These Boundaries

These boundaries were chosen because each area has distinct clinical reasoning patterns, different temporal scales (acute stone episodes versus decades-long cancer surveillance), different outcome measures, and different regulatory requirements. A urologist thinking about stone composition analysis uses fundamentally different mental models than one planning a robotic prostatectomy, even though both are urologists.

## Boundary Enforcement

Each bounded context enforces its boundary by defining clear interfaces for incoming and outgoing information. The Clinical Assessment Context publishes diagnostic profiles but does not dictate treatment decisions. The Oncologic Context consumes pathology results but translates them into its own staging model rather than using raw pathology data structures directly.

## Shared Concepts Across Boundaries

Some concepts appear in multiple contexts but carry different meanings. "Patient" in the Clinical Assessment Context is a diagnostic subject with symptoms and test results. "Patient" in the Surgical Management Context is a surgical candidate with comorbidities and anesthetic risk. Each context maintains its own patient model appropriate to its needs, and translation occurs at the boundaries.

## Context Ownership

Each bounded context has clear ownership. The team responsible for a context owns its model, its ubiquitous language, and its business rules. Changes to a context's model are made by its owning team in consultation with domain experts from that subspecialty. This prevents the model from being diluted by concerns from other contexts.

## Evolution of Boundaries

Bounded context boundaries are not permanent. As the urology domain evolves, new subspecialties may emerge that warrant their own contexts. For example, female pelvic medicine and reconstructive surgery (FPMRS) might evolve from a subspecialty within the Incontinence Management Context into its own bounded context as the model grows in complexity.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 7.
- Campbell-Walsh-Wein Urology. 12th Edition. Elsevier, 2020.
