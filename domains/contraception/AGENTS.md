# Contraception Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to contraception services and family planning, encompassing contraceptive method selection and counseling, prescription and dispensing workflows, long-acting reversible contraception (LARC) management, patient eligibility and medical history screening, follow-up monitoring and side effect management, and reproductive health education.

## Bounded Contexts

1. Contraceptive Counseling - Manages the patient-centered process of discussing contraceptive options, effectiveness rates, side effects, and lifestyle considerations to support informed method selection.
2. Method Prescribing - Handles the clinical workflow of prescribing contraceptive methods, including medical eligibility assessment using standardized criteria, drug interactions, and contraindication screening.
3. LARC Management - Governs the insertion, monitoring, and removal of long-acting reversible contraceptive devices such as intrauterine devices (IUDs) and subdermal implants, including scheduling and procedural documentation.
4. Follow-Up and Monitoring - Tracks ongoing patient follow-up visits, side effect reporting, method satisfaction, continuation rates, and clinical reassessment for method switching or discontinuation.

## Domain Principles

- Model using shared ubiquitous language between reproductive health clinicians, pharmacists, family planning counselors, patients, and system developers.
- Group the system into bounded contexts reflecting the distinct workflows of counseling, prescribing, device management, and follow-up care.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow the WHO Medical Eligibility Criteria (MEC) for Contraceptive Use, UK Faculty of Sexual and Reproductive Healthcare (FSRH) guidelines, and CDC Selected Practice Recommendations.

## Key Topics

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
- contraceptive-counseling
- method-prescribing
- larc-management
- follow-up-and-monitoring
- event-driven-architecture
- integration-patterns
- contraception-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- World Health Organization. Medical Eligibility Criteria for Contraceptive Use. 5th ed. WHO, 2015.
- Faculty of Sexual and Reproductive Healthcare. FSRH Clinical Guidelines. FSRH, various years.
- Hatcher, Robert A., et al. Contraceptive Technology. 21st ed. Managing Contraception, 2018.
