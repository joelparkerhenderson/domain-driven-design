# Practice Management Context - Dental Domain

## Overview

The Practice Management Context handles the administrative and business operations that support clinical dental care delivery. This context manages appointment scheduling, insurance verification, claims processing, and patient recall systems. It operates as a supporting context that provides services to all clinical bounded contexts through an open host service interface. While not directly involved in clinical decision-making, this context is essential for the operational viability of the dental practice.

## Core Responsibilities

### Appointment Scheduling

Appointment scheduling manages the allocation of provider time, operatory rooms, and support staff across the practice day. The scheduling system accounts for:

- Provider availability including work schedule, break times, and blocked time for administrative tasks.
- Operatory assignment based on procedure requirements (some procedures require specific equipment).
- Procedure duration estimates based on CDT code and tooth count (a single-surface filling requires less time than a multi-unit bridge preparation).
- Buffer time between appointments for room turnover and sterilization.
- Patient preferences for appointment days, times, and providers.

The context manages appointment lifecycle states: requested, scheduled, confirmed, checked-in, in-progress, completed, cancelled, and no-show. Each state transition may trigger downstream actions such as operatory preparation, insurance verification, or recall rescheduling.

Scheduling for orthodontic adjustment visits follows a recurring pattern (typically every 4-8 weeks) that the context manages as a series rather than individual appointments.

### Insurance Verification

Insurance verification confirms a patient's dental benefit eligibility and retrieves current coverage details before treatment begins. The verification process determines:

- Active coverage status and effective dates.
- Benefit maximums (annual and lifetime for orthodontics).
- Deductible amounts (individual and family) and amounts applied to date.
- Coverage percentages by procedure category (typically 100% preventive, 80% basic, 50% major).
- Frequency limitations (prophylaxis every 6 months, bitewing radiographs every 12 months, panoramic every 3-5 years).
- Waiting periods for new coverage (some plans exclude major services for the first 12 months).
- Pre-authorization requirements for specific procedures.
- Coordination of benefits when a patient has dual coverage.

Verification results are communicated to the Treatment Planning Context to enable accurate cost estimation.

### Claims Processing

Claims processing manages the lifecycle of insurance claim submissions from creation through payment or denial resolution. The process includes:

- Claim creation with appropriate CDT procedure codes, tooth numbers, surface designations, and provider information.
- Attachment of required documentation (radiographs, narratives, periodontal charting) for specific procedure types.
- Electronic claim submission via ANSI X12 837D transaction format through clearinghouse connections.
- Tracking of claim status through submission, acknowledgment, pending, and adjudication stages.
- Payment posting when explanation of benefits (EOB) is received.
- Denial management with appeal preparation when claims are denied.
- Secondary claim submission when coordination of benefits applies.

The context maintains the anti-corruption layer that translates between internal domain models and the X12 transaction formats required by insurance clearinghouses.

### Patient Recall

Patient recall manages the systematic process of scheduling patients for periodic preventive care and follow-up appointments. The recall system:

- Assigns recall intervals based on clinical risk assessment (3, 4, or 6 months).
- Generates recall due lists for upcoming periods.
- Manages multi-channel patient outreach (mail, phone, text, email) with escalating contact sequences.
- Tracks patient response and appointment scheduling resulting from recall outreach.
- Identifies patients who are overdue for recall and escalates to clinical staff.
- Distinguishes between prophylaxis recall (preventive) and periodontal maintenance recall (therapeutic).

The recall system coordinates with clinical risk assessments from the Clinical Examination and Periodontal Care contexts to ensure recall intervals match current clinical recommendations.

## Aggregate Roots

**Patient Account**: Manages the administrative relationship with each patient including demographics, insurance coverage, appointment history, and financial ledger. Enforces that claims require verified insurance and appointments require valid provider-operatory combinations.

**Insurance Claim**: Tracks a single claim through its lifecycle from creation to resolution. Enforces CDT code validity and claim amount consistency.

## Key Entities

- Appointment: A scheduled patient visit with provider, operatory, date, time, and status.
- Insurance Policy: A patient's dental coverage with carrier, benefits, and limitations.
- Recall Schedule: A patient's assigned recall interval and next due date.

## Key Value Objects

- Appointment Slot: Date, time, duration, provider, and operatory combination.
- Fee Amount: Monetary amount for a dental service.
- Insurance Benefit: Coverage determination with breakdown by category.
- CDT Procedure Code: Standardized procedure identifier for claims.
- Recall Interval: Prescribed months between preventive visits.

## Domain Events Produced

- AppointmentScheduled: Notifies clinical contexts of upcoming patient visits.
- InsuranceVerified: Provides benefit details for cost estimation.
- ClaimSubmitted: Records claim submission for tracking.
- ClaimAdjudicated: Reports payment or denial determination.

## Domain Events Consumed

- RestorationCompleted: Triggers billing for completed restorative procedures.
- EndodonticTreatmentCompleted: Triggers billing for completed endodontic procedures.
- ScalingAndRootPlaningCompleted: Triggers billing for periodontal procedures.
- OrthodonticCaseStarted: Initiates recurring adjustment appointment scheduling.
- PeriodontalMaintenanceScheduled: Configures periodontal recall intervals.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Dental Association. CDT: Current Dental Terminology. ADA, updated annually.
- ANSI ASC X12. 837D Health Care Claim: Dental. X12 Standards.
