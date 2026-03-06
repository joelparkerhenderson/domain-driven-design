# Domain Services in Ophthalmology

## Overview

Domain services are stateless operations that encapsulate domain logic which does not
naturally belong to a single entity or value object. In the ophthalmology domain,
domain services coordinate operations that span multiple aggregates, perform complex
clinical calculations, or implement business rules that involve collaboration between
domain objects. Domain services are named using the ubiquitous language and represent
meaningful clinical operations.

## Domain Service Principles

Domain services in the ophthalmology domain follow these principles:

- Statelessness: domain services do not maintain state between invocations. All
  required data is passed as parameters or retrieved through repositories.
- Domain logic only: domain services contain pure business logic without infrastructure
  concerns such as database access, network communication, or UI rendering.
- Named operations: each domain service method represents a single, well-defined
  clinical operation from the ubiquitous language.
- Aggregate coordination: domain services orchestrate interactions between aggregates
  that cannot be handled within a single aggregate boundary.

## Domain Services by Bounded Context

### Clinical Examination Context

**ExaminationCompletionService**: Validates that all required components of a
comprehensive eye examination have been documented before finalizing the examination.
Checks that visual acuity has been recorded for both eyes, IOP has been measured, and
appropriate examination elements (slit lamp, fundoscopy) have been performed based on
the examination type and clinical indication.

**ReferralDeterminationService**: Analyzes examination findings to determine whether
referral to a subspecialty context is indicated. Evaluates IOP and optic nerve findings
for glaucoma referral, retinal findings for retinal care referral, and lens opacity
for surgical management referral. Applies clinical guidelines to generate referral
recommendations.

### Diagnostic Imaging Context

**ImagingCorrelationService**: Correlates imaging findings across multiple studies and
modalities for the same patient. Compares current OCT measurements with prior scans to
identify changes, correlates visual field results with OCT RNFL thickness, and aligns
fundus photography with angiography findings to provide a comprehensive imaging summary.

**ImageQualityAssessmentService**: Evaluates the technical quality of acquired imaging
data against established criteria. Assesses signal strength for OCT scans, reliability
indices for visual field tests, and image clarity for fundus photographs. Flags studies
that require repeat acquisition.

### Surgical Management Context

**IOLCalculationService**: Performs intraocular lens power calculation using biometry
data (axial length, keratometry, anterior chamber depth) and selected formulas (SRK/T,
Haigis, Barrett Universal II, Hill-RBF). Accepts target refraction and returns
recommended IOL models and powers with predicted refractive outcomes.

**SurgicalRiskAssessmentService**: Evaluates surgical risk based on patient factors
(age, systemic conditions, ocular comorbidities), procedure complexity, and historical
outcomes data. Generates a structured risk assessment with risk score, contributing
factors, and risk mitigation recommendations.

**SurgicalOutcomeAnalysisService**: Compares actual surgical outcomes against predicted
outcomes and population benchmarks. Analyzes visual acuity outcomes, refractive accuracy,
and complication rates for quality improvement purposes.

### Refractive Services Context

**CandidacyEvaluationService**: Evaluates a patient's suitability for refractive surgery
by analyzing corneal topography, pachymetry, wavefront data, and ocular health against
established candidacy criteria for LASIK, PRK, and implantable lens procedures. Returns
a structured candidacy determination with rationale.

**NomogramAdjustmentService**: Applies surgeon-specific nomogram adjustments to laser
treatment parameters based on historical outcomes. Analyzes the surgeon's outcome data
to recommend treatment modifications for improved refractive accuracy.

### Glaucoma Management Context

**ProgressionAnalysisService**: Analyzes longitudinal data (IOP trends, visual field
series, OCT RNFL trends) to determine whether a patient's glaucoma is progressing. Applies
statistical methods (trend analysis, event analysis, guided progression analysis) to
differentiate true progression from measurement variability.

**TreatmentEscalationService**: Evaluates whether a patient's current glaucoma treatment
is achieving target IOP and maintaining visual field stability. Recommends treatment
modifications based on clinical guidelines, including medication changes, laser
treatment, or surgical referral.

**TargetIOPCalculationService**: Calculates an appropriate target IOP for a glaucoma
patient based on disease stage, rate of progression, life expectancy, baseline IOP,
and risk factors. Applies evidence-based formulas to determine the target pressure
reduction needed to slow progression.

### Retinal Care Context

**RetinalSeverityClassificationService**: Classifies the severity of retinal conditions
using established grading systems. Applies ETDRS classification for diabetic retinopathy,
AREDS classification for AMD, and standard staging for other retinal conditions based
on clinical and imaging findings.

**InjectionSchedulingService**: Determines the appropriate injection schedule for
intravitreal therapy based on treatment protocol (fixed dosing, PRN, treat-and-extend),
treatment response, and disease activity markers. Calculates the next injection date
and monitoring schedule.

**TreatmentResponseAssessmentService**: Evaluates the response to retinal treatment
by comparing pre- and post-treatment measurements including visual acuity, OCT central
macular thickness, and disease activity markers. Classifies response as improved,
stable, or worsened, and recommends protocol adjustments.

## Cross-Context Domain Services

**ClinicalDecisionSupportService**: Aggregates findings from multiple bounded contexts
to provide clinical decision support. This service is an application-level service that
coordinates domain services from individual contexts rather than a single-context domain
service.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5.
