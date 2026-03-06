# Pre-Operative Assessment Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to pre-operative assessment systems. Pre-operative assessment is the structured clinical process of evaluating a patient's medical fitness for surgery and anaesthesia before a planned procedure. By applying DDD principles, we create a model that captures the multidisciplinary nature of pre-operative evaluation, from initial screening and risk stratification through anaesthetic planning, investigation ordering, patient optimization, and final surgical clearance.

The pre-operative assessment domain serves as a critical bridge between the decision to operate and the day of surgery. It involves anaesthetists, surgeons, pre-assessment nurses, and perioperative medicine physicians working together to identify comorbidities, calculate perioperative risk, order evidence-based investigations, optimize modifiable risk factors, and confirm surgical readiness. The domain model organizes these workflows into bounded contexts that reflect the sequential and iterative nature of the pre-operative pathway, ensuring patients are adequately prepared to achieve the best possible surgical outcomes.

## Bounded Contexts

1. **Patient Screening and Triage Context** - Manages the initial evaluation of surgical referrals to determine the appropriate level of pre-operative assessment. This context applies validated triage criteria based on patient comorbidity burden and surgical complexity to allocate patients to self-completed questionnaires, telephone assessments, nurse-led clinic appointments, or consultant-led multidisciplinary reviews.

2. **Medical History and Risk Stratification Context** - Handles the comprehensive collection of medical, surgical, medication, allergy, family, and social history, and applies validated risk scoring systems including the ASA Physical Status Classification, Revised Cardiac Risk Index (RCRI), and procedure-specific surgical risk calculators to produce a structured perioperative risk profile.

3. **Anaesthetic Evaluation Context** - Manages the anaesthetist's assessment of airway anatomy (Mallampati classification, thyromental distance, mouth opening), cardiopulmonary functional capacity (metabolic equivalents), previous anaesthetic history, and medication management requirements. This context produces an anaesthetic plan specifying technique, monitoring, and perioperative medication adjustments.

4. **Pre-Operative Investigations Context** - Handles the evidence-based ordering, tracking, and interpretation of diagnostic tests guided by NICE NG45 guidelines and patient-specific risk factors rather than routine blanket testing. This context manages laboratory investigations, electrocardiograms, imaging studies, and specialist referrals triggered by abnormal results.

5. **Patient Optimization Context** - Manages the identification and treatment of modifiable risk factors prior to surgery, including anaemia correction through iron supplementation or transfusion planning, glycaemic optimization for diabetic patients, blood pressure management, smoking cessation support, nutritional optimization, and prehabilitation exercise programs.

6. **Surgical Readiness Clearance Context** - Manages the final determination of a patient's fitness to proceed with surgery, confirming that all required investigations are complete and satisfactory, optimization targets have been met, informed consent is documented, fasting instructions are communicated, and the surgical pathway (day surgery, planned admission, critical care bed) is confirmed.

## Strategic Design

- **Subdomains** classify areas by strategic importance: medical history and risk stratification, and anaesthetic evaluation as core subdomains; pre-operative investigations and patient optimization as supporting subdomains; scheduling, appointment booking, and patient communication as generic subdomains.
- **Context Map** defines relationships between bounded contexts, with a sequential flow from screening through risk assessment, anaesthetic evaluation, investigation, optimization, and final clearance, with feedback loops when new findings alter the risk profile.
- **Anti-Corruption Layers** protect bounded contexts from external systems such as electronic health records, surgical scheduling systems, laboratory information systems, and blood bank management systems.

## Tactical Design

- **Aggregates** enforce consistency boundaries around related pre-operative concepts such as pre-assessment cases, risk profiles, anaesthetic plans, and optimization protocols.
- **Entities** represent domain objects with unique identity, such as patients, surgical procedures, investigations, and optimization targets.
- **Value Objects** capture immutable classifications, scores, and measurements such as ASA grades, Mallampati classifications, RCRI scores, and functional capacity estimates.
- **Domain Events** signal clinically significant occurrences such as triage completion, risk stratification, investigation result receipt, optimization target achievement, and surgical clearance.
- **Repositories** abstract the persistence of aggregate roots within each bounded context.
- **Domain Services** encapsulate operations spanning multiple aggregates, such as evidence-based investigation recommendation engines, risk score calculators, and optimization protocol assignment logic.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- National Institute for Health and Care Excellence (NICE). *Routine Preoperative Tests for Elective Surgery (NG45)*. NICE, 2016.
- Association of Anaesthetists of Great Britain and Ireland. *Pre-operative Assessment and Patient Preparation: The Role of the Anaesthetist*. AAGBI, 2010.
- Grocott, Michael P.W. et al. *Perioperative Increase in Global Blood Flow to Explicit Defined Goals and Outcomes After Surgery*. Cochrane Database of Systematic Reviews, 2012.
