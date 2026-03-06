# Surgical Services Context

## Overview

The Surgical Services Context is a bounded context within the gynecology domain that governs surgical interventions. It manages the complete lifecycle of a surgical case from referral through postoperative recovery, encompassing consent, operative planning, perioperative protocol management, and outcome tracking.

## Scope and Responsibility

This context owns the surgical case model. Its responsibilities include:

- Receiving and processing surgical referrals from the Clinical Assessment Context.
- Managing the informed consent process, documenting patient understanding of risks, benefits, and alternatives.
- Developing operative plans specifying surgical approach, procedure type, and anticipated requirements.
- Defining and tracking perioperative protocols including preoperative preparation, intraoperative standards, and postoperative recovery milestones.
- Recording surgical outcomes including procedure details, complications, and estimated blood loss.
- Managing postoperative follow-up scheduling and milestone tracking.

## Ubiquitous Language

Key terms within this context:

- Surgical Case: The complete lifecycle of a surgical intervention from referral through recovery closure.
- Surgical Consent: A documented agreement obtained after informing the patient about the procedure, risks, benefits, and alternatives.
- Operative Plan: A detailed specification of the intended surgical procedure, approach, and equipment requirements.
- Perioperative Protocol: The standardized sequence of preoperative, intraoperative, and postoperative care steps.
- Minimally Invasive Surgery: Techniques using small incisions, including laparoscopy, hysteroscopy, and robotic-assisted approaches.
- Hysterectomy: Removal of the uterus, classified by approach and extent.
- Pelvic Floor Repair: Reconstructive surgery addressing prolapse or incontinence.
- Surgical Outcome: The documented result of the procedure including complications and recovery trajectory.

## Aggregate: Surgical Case

The Surgical Case is the primary aggregate root. It contains:

- SurgicalConsent value object documenting the consent process and patient understanding.
- OperativePlan value object specifying the intended procedure, approach, and requirements.
- PerioperativeProtocol entity tracking preoperative, intraoperative, and postoperative steps.
- PostoperativeFollowUp entity managing recovery milestones and follow-up visits.
- SurgicalOutcome value object recording procedure results.

Key invariants:

- Surgery cannot be scheduled without documented consent.
- The operative plan must specify at minimum the procedure type and surgical approach.
- Postoperative follow-up cannot be initiated before surgery is marked as completed.
- A surgical case cannot be closed until all required postoperative milestones are addressed.
- Consent must be re-obtained if the operative plan changes materially after initial consent.

## Domain Events Published

- **SurgeryScheduled**: Published when consent is documented and a surgical date is confirmed. Consumed by Patient Education Context for preoperative education.
- **SurgeryCompleted**: Published when the procedure is performed and outcome is documented. Consumed by Clinical Assessment Context for postoperative follow-up scheduling.
- **PostoperativeComplicationReported**: Published when a postoperative complication is identified. Consumed by Clinical Assessment Context for urgent follow-up.

## Domain Events Consumed

- **ClinicalReferralCreated** (from Clinical Assessment): Triggers creation of a new surgical case.
- **LaborPlanFinalized** (from Prenatal Care): Triggers surgical case creation when cesarean delivery is planned.
- **EducationSessionCompleted** (from Patient Education): Records that preoperative or postoperative education has been delivered.

## Domain Services

- **SurgicalRiskAssessmentService**: Calculates perioperative risk based on patient comorbidities, procedure complexity, and anesthesia classification.
- **ProcedureSelectionService**: Evaluates clinical factors to recommend optimal surgical approach (minimally invasive versus open).

## Value Objects

- ProcedureType, AnesthesiaClassification, SurgicalOutcome, RecoveryMilestone.

## Integration Points

- Downstream from Clinical Assessment Context via ClinicalReferralCreated events.
- Downstream from Prenatal Care Context via LaborPlanFinalized events.
- Upstream to Clinical Assessment Context via SurgeryCompleted events.
- Anti-corruption layer for integration with external operating room scheduling systems.
- Anti-corruption layer for integration with anesthesiology information systems.

## Procedure Types Modeled

- Diagnostic laparoscopy and hysteroscopy.
- Total and subtotal hysterectomy (laparoscopic, vaginal, abdominal, robotic-assisted).
- Myomectomy (removal of uterine fibroids).
- Pelvic floor reconstruction including anterior/posterior colporrhaphy and sacrocolpopexy.
- Endometrial ablation.
- Ovarian cystectomy and oophorectomy.
- Cesarean delivery (coordinated with Prenatal Care Context).

## Subdomain Classification

Surgical Services is a supporting subdomain. The surgical case lifecycle follows patterns common across surgical specialties, though the specific procedures are gynecology-specific.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- ACOG. Committee Opinion No. 701: Choosing the Route of Hysterectomy for Benign Disease, 2017.
- AAGL. Practice Guidelines for Laparoscopic Surgery.
