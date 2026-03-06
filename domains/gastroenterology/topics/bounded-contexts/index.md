# Bounded Contexts in Gastroenterology

## Overview

A bounded context defines a logical boundary within which a particular domain model is
consistent and unambiguous. In the gastroenterology domain, six bounded contexts
partition the clinical landscape into coherent areas where terminology, business rules,
and model structure are internally consistent.

## The Six Bounded Contexts

### 1. Clinical Assessment Context

This context owns the initial and ongoing evaluation of patients with gastrointestinal
complaints. It manages symptom documentation, physical examination findings, laboratory
study ordering and result interpretation, and imaging referrals. The key aggregate is
PatientEncounter, which captures the full scope of a clinical visit. Within this context,
a "finding" refers specifically to a clinical observation from history or examination,
distinguishing it from an endoscopic finding or a pathology finding in other contexts.

### 2. Endoscopic Procedures Context

This context manages the complete lifecycle of endoscopic procedures including EGD,
colonoscopy, ERCP, EUS, and capsule endoscopy. It covers scheduling, informed consent,
sedation documentation, procedural findings, therapeutic interventions, specimen
collection, and pathology result tracking. The EndoscopicProcedure aggregate ensures
that all procedural documentation remains consistent. A "finding" here refers
specifically to a visual observation made during endoscopy.

### 3. Inflammatory Disease Context

This context handles the longitudinal management of inflammatory bowel disease,
including Crohn's disease and ulcerative colitis. It tracks disease classification,
activity scoring (Harvey-Bradshaw Index, Mayo Score), biologic therapy regimens,
immunomodulator management, and flare episodes. The IBDDiagnosis aggregate maintains
disease state over time. Treatment decisions in this context follow IBD-specific
guidelines distinct from treatment logic in other contexts.

### 4. Hepatology Context

This context manages liver disease diagnosis, hepatitis tracking, cirrhosis staging
using Child-Pugh and MELD scoring, complication management (ascites, variceal bleeding,
hepatic encephalopathy), and transplant evaluation workflows. The LiverDiseaseDiagnosis
aggregate tracks disease progression. "Staging" in this context specifically refers to
the severity classification of liver disease, not cancer staging.

### 5. Motility Disorders Context

This context handles motility diagnostic studies including esophageal and anorectal
manometry, ambulatory pH monitoring, and impedance testing. It also manages GERD
treatment protocols and functional GI disorder classification per Rome IV criteria.
The ManometryStudy aggregate captures pressure measurement data. "Normal values"
in this context refer to established motility reference ranges.

### 6. Nutrition Management Context

This context manages nutritional assessment, enteral and parenteral nutrition planning,
celiac disease monitoring, food intolerance evaluation, and dietary intervention
tracking. The NutritionalAssessment aggregate evaluates nutritional status through
anthropometric, biochemical, and clinical indicators. "Intervention" here means a
dietary or nutritional modification, not a procedural intervention.

## Context Boundaries

Each bounded context maintains its own model of shared concepts. A Patient exists in
every context, but each context tracks different aspects. The Clinical Assessment
Context tracks symptoms and examination findings. The Endoscopic Procedures Context
tracks procedure history and sedation tolerance. The Hepatology Context tracks liver
function trends. These are different models of the same real-world entity, and each
context owns its perspective independently.

## Boundary Enforcement

Bounded context boundaries are enforced through explicit integration points. No context
directly accesses another context's internal model. Communication occurs through domain
events, published language contracts, or anti-corruption layer translations. This
prevents the model corruption that would occur if, for example, the Endoscopic Procedures
Context directly manipulated IBD disease activity scores.

## Team Alignment

In practice, bounded contexts often align with clinical team structures. The endoscopy
suite team owns the Endoscopic Procedures Context. The IBD clinic team owns the
Inflammatory Disease Context. The hepatology service owns the Hepatology Context. This
alignment ensures that domain experts have direct influence over their context's model.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Maintaining Model Integrity.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 6: Bounded Contexts.
