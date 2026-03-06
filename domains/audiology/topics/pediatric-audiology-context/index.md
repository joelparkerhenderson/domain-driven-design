# Pediatric Audiology Context

## Overview

The Pediatric Audiology Context addresses the specialized needs of hearing assessment and intervention for children from birth through adolescence. This bounded context recognizes that pediatric audiology requires fundamentally different approaches from adult audiology: age-appropriate testing methods, family-centered care models, developmental monitoring, and coordination with early intervention services. The domain model adapts core audiologic concepts for the unique clinical, developmental, and familial considerations of pediatric patients.

## Strategic Importance

As a supporting subdomain, the Pediatric Audiology Context builds upon the shared kernel with the Hearing Assessment Context while adding pediatric-specific extensions. Early identification and intervention for childhood hearing loss have profound impacts on speech, language, and cognitive development. The domain model must enforce the timelines and protocols defined by Early Hearing Detection and Intervention (EHDI) programs and support the longitudinal tracking of developmental outcomes.

## Ubiquitous Language

Key terms within this bounded context include: newborn hearing screening, universal newborn hearing screening (UNHS), automated auditory brainstem response (AABR), otoacoustic emissions screening (OAE), pass/refer result, diagnostic ABR, visual reinforcement audiometry (VRA), conditioned play audiometry (CPA), behavioral observation audiometry (BOA), ear-specific thresholds, pediatric fitting, DSL v5 prescriptive formula, earmold, electroacoustic verification, developmental milestone, speech-language milestone, early intervention, Individualized Family Service Plan (IFSP), family-centered care, parent coaching, and EHDI guidelines.

## Aggregates

### Pediatric Patient

The Pediatric Patient is the central aggregate, rooted by the PediatricPatient entity. It extends the standard patient model with DevelopmentalMilestone value objects (tracking auditory, speech, and language milestones against age norms), ScreeningResult value objects (initial and follow-up screening outcomes), and FamilyEngagementPlan entities (documenting family participation and coaching activities). The aggregate enforces age-appropriate testing method selection and EHDI timeline compliance.

### Screening Event

The Screening Event aggregate captures newborn hearing screening occurrences. Rooted by the ScreeningEvent entity, it contains ScreeningResult value objects (pass/refer for each ear, screening method used), ScreeningEnvironment value objects (noise level, infant state), and follow-up tracking data. The aggregate enforces that referred results trigger appropriate diagnostic follow-up within mandated timeframes.

### Pediatric Evaluation

The Pediatric Evaluation aggregate extends the core AudiometricEvaluation with pediatric-specific elements. It adds the testing method used (VRA, CPA, BOA, ABR), the reliability rating of behavioral responses, and developmental context. The aggregate enforces that the testing method is appropriate for the child's developmental age and that reliability is documented.

## Value Objects

ScreeningResult captures the outcome of a hearing screening (pass, refer, or incomplete) along with the method, ear, and screening conditions. DevelopmentalMilestone records a specific developmental benchmark with expected age, observed age, and status (met, delayed, not yet assessed). PediatricFittingParameters extends standard fitting parameters with pediatric corrections (real ear to coupler difference, age-dependent ear canal acoustics). EarmoldSpecification documents the earmold characteristics (material, style, venting) that must be frequently updated as the child grows.

## Domain Events

NewbornScreeningReferred is published when a screening produces a refer result, triggering the diagnostic follow-up cascade. This is a time-critical event: EHDI guidelines specify that diagnostic evaluation should occur by three months of age. DevelopmentalMilestoneDelayed is published when a child misses an expected milestone, triggering clinical review and potential intervention adjustment. PediatricFittingCompleted is published when a pediatric fitting is finalized, carrying additional information about electroacoustic verification and family training. ScreeningPassed is published to confirm normal screening results and close the screening workflow.

## Domain Services

The DevelopmentalMonitoringService compares the child's current developmental status against age-appropriate norms and identifies delays requiring attention. The ScreeningFollowUpService tracks EHDI timeline compliance, monitoring the one-three-six benchmarks (screening by one month, diagnosis by three months, intervention by six months). The PediatricFittingCalculationService applies the DSL v5 prescriptive formula with age-appropriate real ear to coupler difference (RECD) values to generate pediatric fitting targets.

## Business Rules

EHDI compliance is a primary business rule: all newborns must be screened before one month of age, diagnostic evaluation must be completed by three months for referred infants, and early intervention must begin by six months for diagnosed children. Pediatric hearing aid fittings must use the DSL v5 prescriptive formula (or equivalent validated pediatric formula) with measured or age-appropriate RECD values. Electroacoustic verification using a hearing aid test box with coupler measurements is required for all pediatric fittings (real ear measurement with probe microphones may not be feasible for very young children). Earmold fit must be reassessed regularly to accommodate ear canal growth. Developmental milestone monitoring must occur at recommended intervals.

## Family-Centered Care

The domain model embeds family-centered care principles. The PediatricPatient aggregate includes a FamilyEngagementPlan that tracks family participation in the child's hearing healthcare. Parent coaching sessions are modeled as intervention activities. Family members are recognized as essential participants in the child's auditory development, and their engagement is tracked as a factor in outcome measurement.

## Age-Appropriate Testing Methods

The domain model enforces age-appropriate testing method selection. Newborns and infants under six months are evaluated using ABR and OAE (objective measures). Infants from approximately six months to two years are evaluated using visual reinforcement audiometry (VRA). Children from approximately two to five years are evaluated using conditioned play audiometry (CPA). Children over five years can typically be evaluated using conventional audiometry. The domain model validates that the selected method is appropriate for the child's developmental age.

## Integration with Other Contexts

The Pediatric Audiology Context shares a kernel with the Hearing Assessment Context for core audiometric concepts. It acts as a customer of the Device Management Context for pediatric device fitting, communicating pediatric-specific constraints. It supplies developmental and family context to the Rehabilitation Context for designing age-appropriate intervention programs. All clinical activities are published to the Clinical Documentation Context.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Madell, J. R., & Flexer, C. (2014). Pediatric Audiology: Diagnosis, Technology, and Management. 2nd Edition. Thieme.
- Joint Committee on Infant Hearing (JCIH). (2019). Year 2019 Position Statement.
