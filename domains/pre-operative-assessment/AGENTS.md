# Pre-Operative Assessment Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to pre-operative assessment systems, encompassing patient screening and triage, medical history and risk stratification, anaesthetic evaluation, pre-operative investigations, patient optimization, and surgical readiness clearance. Pre-operative assessment is the systematic clinical process of evaluating a patient's fitness for surgery and anaesthesia, identifying modifiable risk factors, and planning perioperative care to reduce complications and improve surgical outcomes.

## Bounded Contexts

1. Patient Screening and Triage Context - Initial evaluation of surgical referrals to determine the appropriate level of pre-operative assessment, ranging from self-completed health questionnaires for low-risk patients to comprehensive multidisciplinary clinic appointments for complex cases, using validated triage criteria and surgical complexity grading.
2. Medical History and Risk Stratification Context - Comprehensive collection and analysis of the patient's medical, surgical, medication, allergy, and social history to calculate perioperative risk using validated scoring systems such as ASA Physical Status Classification, Revised Cardiac Risk Index (RCRI), and surgical risk calculators.
3. Anaesthetic Evaluation Context - Assessment of airway, cardiopulmonary reserve, and anaesthetic suitability, including airway examination (Mallampati score, thyromental distance), functional capacity assessment (METs), medication management plans (anticoagulant bridging, insulin adjustment), and anaesthetic technique planning.
4. Pre-Operative Investigations Context - Evidence-based ordering and interpretation of laboratory tests (full blood count, coagulation, renal function, HbA1c), electrocardiograms, chest radiographs, echocardiograms, and pulmonary function tests, guided by NICE guidelines and patient risk factors rather than routine blanket testing.
5. Patient Optimization Context - Management of modifiable risk factors to improve surgical outcomes, including smoking cessation, anaemia correction (iron therapy, transfusion planning), glycaemic control, blood pressure optimization, nutrition improvement, prehabilitation exercise programs, and medication adjustment.
6. Surgical Readiness Clearance Context - Final determination of a patient's fitness to proceed with surgery on the planned date, including confirmation that all investigations are complete, optimization targets are met, consent is documented, fasting instructions are issued, and the surgical pathway is confirmed.

## Domain Principles

- Model using shared ubiquitous language between anaesthetists, surgeons, pre-assessment nurses, physicians, and software developers.
- Group the system into bounded contexts reflecting the distinct clinical workflows of screening, risk assessment, anaesthetic planning, investigation, optimization, and clearance.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow NICE pre-operative testing guidelines, Association of Anaesthetists of Great Britain and Ireland (AAGBI) recommendations, and Enhanced Recovery After Surgery (ERAS) protocols.
- Maintain patient safety, evidence-based investigation ordering, and timely surgical pathway progression as cross-cutting concerns.

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
- patient-screening-triage-context
- medical-history-risk-stratification-context
- anaesthetic-evaluation-context
- pre-operative-investigations-context
- patient-optimization-context
- surgical-readiness-clearance-context
- event-driven-architecture
- integration-patterns
- pre-operative-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- National Institute for Health and Care Excellence (NICE). Routine Preoperative Tests for Elective Surgery (NG45). NICE, 2016.
- Association of Anaesthetists of Great Britain and Ireland. Pre-operative Assessment and Patient Preparation. AAGBI, 2010.
- Grocott, Michael P.W. et al. Perioperative Increase in Global Blood Flow to Explicit Defined Goals and Outcomes After Surgery. Cochrane Database of Systematic Reviews, 2012.
