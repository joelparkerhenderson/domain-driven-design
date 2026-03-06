# Gynecology Standards

## Overview

Gynecology standards are clinical guidelines, classification systems, and evidence-based protocols established by professional organizations and regulatory bodies. In the Domain-Driven Design context, these standards inform the design of value objects, domain services, and business rules within each bounded context. They represent codified clinical knowledge that the domain model must faithfully encode.

## Purpose

Gynecological practice is governed by extensive clinical standards that dictate screening intervals, diagnostic classifications, treatment eligibility criteria, and quality metrics. A DDD model for gynecology must encode these standards as first-class domain concepts rather than treating them as configuration data or external lookups. This ensures that the domain model enforces clinical correctness by construction.

## Screening Standards

### Cervical Cancer Screening

- USPSTF recommends cervical cancer screening for women aged 21-65 using Pap smear cytology every 3 years (ages 21-29) or Pap with HPV co-testing every 5 years (ages 30-65).
- ACOG endorses these intervals with additional guidance for specific populations.
- The Bethesda System provides standardized terminology for reporting cervical cytology results: NILM (Negative for Intraepithelial Lesion or Malignancy), ASC-US, LSIL, HSIL, ASC-H, AGC.
- ASCCP Risk-Based Management Guidelines determine follow-up actions based on current and prior results.
- These standards are encoded as value objects (BethesdaClassification, ScreeningInterval) and domain service logic (ScreeningScheduleService, ResultInterpretationService).

### Breast Cancer Screening

- USPSTF recommends biennial mammography for women aged 50-74.
- ACS recommends annual mammography starting at age 45, transitioning to biennial at age 55.
- ACR BI-RADS (Breast Imaging Reporting and Data System) standardizes mammography result reporting with categories 0-6.
- These standards inform the ScreeningScheduleService and result classification value objects.

### STI Screening

- CDC recommends annual chlamydia and gonorrhea screening for sexually active women under 25.
- USPSTF recommends syphilis screening for at-risk populations.
- Screening intervals and target populations are encoded in the ScreeningScheduleService.

## Contraception Standards

### WHO Medical Eligibility Criteria

- The WHO MEC classifies contraceptive methods into four eligibility categories for each medical condition:
  - Category 1: No restriction on use.
  - Category 2: Advantages generally outweigh risks.
  - Category 3: Risks generally outweigh advantages.
  - Category 4: Unacceptable health risk (method contraindicated).
- These categories are encoded as value objects and enforced as invariants in the Reproductive Health Context. The ContraceptionEligibilityService applies MEC criteria to patient medical histories.

### ACOG Contraception Guidelines

- ACOG Practice Bulletins provide specific guidance on long-acting reversible contraception (LARC), hormonal contraception, and emergency contraception.
- These guidelines supplement WHO MEC with US-specific clinical recommendations.

## Prenatal Care Standards

### ACOG Perinatal Guidelines

- Guidelines for Perinatal Care (ACOG/AAP) define visit schedules, laboratory testing, ultrasound timing, and risk assessment protocols.
- Standard prenatal visit schedule: monthly through week 28, biweekly weeks 28-36, weekly from week 36 to delivery.
- Required laboratory tests by trimester are encoded in the Prenatal Care Context domain logic.

### Ultrasound Standards

- AIUM (American Institute of Ultrasound in Medicine) defines standards for obstetric ultrasound examinations.
- First-trimester dating scan, second-trimester anatomy scan, and third-trimester growth assessments follow standardized protocols.
- Fetal measurement reference ranges and percentile calculations are encoded as value objects.

## Surgical Standards

### AAGL Minimally Invasive Surgery Guidelines

- AAGL (American Association of Gynecologic Laparoscopists) publishes practice guidelines for laparoscopic and hysteroscopic surgery.
- ACOG Committee Opinion on Choosing the Route of Hysterectomy recommends minimally invasive approaches when feasible.
- These guidelines inform the ProcedureSelectionService and operative plan development.

### Perioperative Standards

- ASA Physical Status Classification System standardizes preoperative risk assessment.
- Enhanced Recovery After Surgery (ERAS) protocols define perioperative care pathways.
- These standards are encoded as value objects (AnesthesiaClassification) and perioperative protocol logic.

## Coding and Classification Standards

- ICD-10-CM for diagnostic coding of gynecological conditions.
- CPT (Current Procedural Terminology) for procedure coding.
- LOINC for laboratory test identification.
- SNOMED CT for clinical terminology standardization.
- These coding standards are encoded as value objects with validation rules.

## How Standards Enter the Domain Model

1. Standards are analyzed by domain experts (clinicians) and domain modelers together.
2. Decision rules from standards become domain service logic.
3. Classification categories from standards become value objects.
4. Screening intervals and visit schedules become domain-level business rules.
5. Eligibility criteria become aggregate invariants.
6. Updates to standards trigger model updates through a defined change management process.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- USPSTF. Recommendation Statements for Cervical Cancer Screening, Breast Cancer Screening, and STI Screening.
- ACOG. Practice Bulletins and Committee Opinions.
- WHO. Medical Eligibility Criteria for Contraceptive Use, 5th edition, 2015.
- ASCCP. Risk-Based Management Consensus Guidelines, 2019.
