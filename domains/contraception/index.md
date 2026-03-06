# Contraception Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to contraception services and family planning. Contraception encompasses the methods, devices, and medications used to prevent pregnancy, along with the clinical workflows for counseling, prescribing, monitoring, and managing contraceptive care. Effective contraception management requires integrating patient preferences, medical eligibility criteria, method-specific clinical protocols, and long-term follow-up to support reproductive autonomy and health outcomes.

## Bounded Contexts

1. **Contraceptive Counseling** - Manages the patient-centered process of discussing available contraceptive options, comparing effectiveness rates, reviewing side effect profiles, and aligning method selection with the patient's reproductive goals, lifestyle, and preferences. This context supports shared decision-making and ensures that counseling sessions are documented.

2. **Method Prescribing** - Handles the clinical workflow of assessing a patient's medical eligibility for specific contraceptive methods using the WHO Medical Eligibility Criteria categories, screening for contraindications and drug interactions, and generating prescriptions. This context ensures that prescribing decisions are evidence-based and documented.

3. **LARC Management** - Governs the clinical management of long-acting reversible contraceptive devices, including intrauterine devices (copper and hormonal) and subdermal implants. This context covers device selection, insertion procedures, post-insertion checks, scheduled monitoring, and planned or unplanned removal, along with procedural documentation and device tracking.

4. **Follow-Up and Monitoring** - Tracks ongoing patient follow-up after method initiation, including scheduled review visits, side effect reporting, satisfaction assessment, continuation tracking, and clinical reassessment for method switching or discontinuation. This context supports proactive management of patient outcomes over time.

5. **Supply and Dispensing** - Manages the inventory, dispensing, and resupply workflows for contraceptive products, including pharmacy dispensing records, repeat prescription management, and coordination between clinical prescribers and dispensing providers.

## Strategic Design

- **Subdomains** classify areas by strategic importance, with counseling and prescribing as core subdomains and LARC management, follow-up, and supply as supporting subdomains.
- **Context Map** defines relationships between bounded contexts, such as the upstream dependency of Method Prescribing on Contraceptive Counseling.
- **Anti-Corruption Layers** protect bounded contexts from external pharmacy, clinical records, and laboratory systems.

## Tactical Design

- **Aggregates** enforce consistency boundaries, such as the Contraceptive Episode aggregate that groups method selection, prescription, and follow-up records.
- **Entities** represent domain objects with unique identity, such as Patient, Contraceptive Method, and Prescription.
- **Value Objects** capture immutable measurements and types, such as MEC Category, Effectiveness Rate, and Dosage.
- **Domain Events** signal significant occurrences, such as MethodPrescribed, DeviceInserted, DeviceRemoved, and MethodSwitched.
- **Repositories** abstract persistence of contraceptive episodes, device records, and follow-up schedules.
- **Domain Services** encapsulate cross-aggregate operations, such as evaluating medical eligibility across multiple contraindication criteria.

## References

- Evans, Eric. *Domain-Driven Design*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- World Health Organization. *Medical Eligibility Criteria for Contraceptive Use*. 5th ed. WHO, 2015.
- Hatcher, Robert A., et al. *Contraceptive Technology*. 21st ed. Managing Contraception, 2018.
- Faculty of Sexual and Reproductive Healthcare. *FSRH Clinical Guidelines*. FSRH, various years.
