# Screening Programs Context

## Overview

The Screening Programs Context is a bounded context within the gynecology domain that manages preventive screening workflows. It encompasses cervical cancer screening (Pap smear and HPV testing), breast cancer screening, sexually transmitted infection screening, and prenatal screening tests. This context operates at both the individual patient level and the population health level, managing screening schedules, result tracking, and abnormal result management pathways.

## Scope and Responsibility

This context owns the screening episode model. Its responsibilities include:

- Maintaining personalized screening schedules based on patient age, risk factors, and guideline recommendations.
- Ordering screening tests and tracking their completion.
- Receiving and classifying screening results using standardized classification systems.
- Triggering follow-up pathways when abnormal results are detected.
- Tracking follow-up completion to ensure no abnormal results are lost to follow-up.
- Generating screening due notifications when intervals elapse.
- Coordinating with the Prenatal Care Context for pregnancy-specific screening tests.

## Ubiquitous Language

Key terms within this context:

- Screening Episode: A single screening event encompassing test ordering, result receipt, and follow-up disposition.
- Cervical Screening: Preventive testing using Pap smear cytology and/or HPV molecular testing to detect precancerous cervical changes.
- Breast Screening: Clinical breast examination and mammography for early breast cancer detection.
- STI Screening: Laboratory testing for sexually transmitted infections guided by risk factors and clinical guidelines.
- Screening Interval: The recommended time between successive screenings, based on risk and guideline protocols.
- Abnormal Result Management: The clinical pathway triggered by an out-of-normal-range screening result.
- Follow-Up Pathway: The sequence of additional tests, referrals, or monitoring steps required after an abnormal result.
- Bethesda Classification: The standardized system for reporting cervical cytology results.

## Aggregate: Screening Episode

The Screening Episode is the primary aggregate root. It contains:

- ScreeningOrder value object specifying the test type, clinical indication, and ordering context.
- ScreeningResult value object capturing the test outcome, classification, and reporting laboratory.
- FollowUpPathway entity managing the sequence of follow-up actions required.

Key invariants:

- A screening result cannot be recorded without a corresponding screening order.
- An abnormal result must trigger creation of a follow-up pathway; episodes with abnormal results cannot be closed without follow-up disposition.
- The screening test type in the result must match the ordered test type.
- Follow-up pathway steps must be executed in the clinically specified sequence.

## Domain Events Published

- **AbnormalResultDetected**: Published when a screening test returns an abnormal result. Consumed by Clinical Assessment Context for diagnostic follow-up and by Patient Education Context for result explanation.
- **ScreeningDue**: Published when a patient's screening interval elapses and a new screening is due. Consumed by Clinical Assessment Context to prompt screening at the next visit.
- **ScreeningCompleted**: Published when a screening episode reaches final disposition (normal result confirmed or follow-up completed).
- **FollowUpOverdue**: Published when a required follow-up action has not been completed within the expected timeframe.

## Domain Events Consumed

- **PregnancyConfirmed** (from Prenatal Care): Triggers initiation of pregnancy-specific screening schedule (glucose tolerance, group B streptococcus, prenatal genetic screening).
- **GestationalMilestoneReached** (from Prenatal Care): Triggers trimester-specific prenatal screening tests.
- **ClinicalReferralCreated** (from Clinical Assessment): Triggers enrollment in a screening program when clinically indicated.

## Domain Services

- **ScreeningScheduleService**: Determines personalized screening intervals by applying ACOG, USPSTF, and ACS guidelines to patient demographics and risk factors.
- **ResultInterpretationService**: Classifies raw screening results using standardized systems (Bethesda for cervical cytology, BI-RADS for mammography) and determines appropriate follow-up pathways using ASCCP management guidelines.

## Value Objects

- ScreeningTestType, ScreeningResult, ScreeningInterval, BethesdaClassification.

## Screening Programs Modeled

- Cervical cancer screening: Pap smear cytology and HPV co-testing per USPSTF and ACOG guidelines.
- Breast cancer screening: Clinical breast examination and mammography per USPSTF and ACS guidelines.
- STI screening: Chlamydia, gonorrhea, syphilis, HIV, and hepatitis B per CDC and USPSTF guidelines.
- Prenatal screening: First-trimester combined screening, quad screen, glucose tolerance, group B streptococcus.

## Integration Points

- Upstream to Clinical Assessment Context via AbnormalResultDetected events.
- Downstream from Prenatal Care Context via PregnancyConfirmed events.
- Anti-corruption layer for integration with external laboratory information systems.
- Anti-corruption layer for integration with external mammography reporting systems.

## Subdomain Classification

Screening Programs is a supporting subdomain. It follows standardized protocols defined by external authorities, but correct implementation is critical for patient safety and public health outcomes.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- USPSTF. Screening for Cervical Cancer: Recommendation Statement, 2018.
- ASCCP. Risk-Based Management Consensus Guidelines for Abnormal Cervical Cancer Screening Tests, 2019.
- Solomon, D. et al. "The 2001 Bethesda System." JAMA, 2002.
