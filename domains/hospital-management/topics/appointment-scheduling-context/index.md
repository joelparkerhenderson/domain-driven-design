# Appointment/Scheduling Context in Hospital Management

## Overview

The Appointment/Scheduling Context manages provider schedules, appointment booking, resource allocation, waitlists, and appointment lifecycle. It ensures that patients can be seen at the right time by the right provider with the right resources, while preventing conflicts and overbooking.

## Core Responsibilities

- **Schedule Management**: Defining provider availability windows, blocking time for meetings or surgeries, managing recurring schedule templates
- **Appointment Booking**: Creating, modifying, and cancelling appointments with conflict detection
- **Resource Allocation**: Reserving rooms, equipment, and support staff for appointments
- **Waitlist Management**: Managing patient waitlists for high-demand providers or procedures
- **Reminder and Follow-Up**: Triggering appointment reminders and tracking no-shows for follow-up

## Aggregates

### Appointment Aggregate

- **Root**: Appointment (identified by AppointmentID)
- **Value objects**: TimeSlot (start, end, duration), AppointmentType (consultation, follow-up, procedure), CancellationReason, Location
- **States**: Requested → Scheduled → Confirmed → CheckedIn → InProgress → Completed / Cancelled / NoShow
- **Invariants**:
  - Appointment time must fall within provider's available hours
  - Cannot schedule in the past
  - Cancellation requires a reason
  - Status transitions follow defined workflow

### Schedule Aggregate

- **Root**: Schedule (identified by Provider NPI + date range)
- **Value objects**: AvailabilityBlock (day, start time, end time, recurrence), BlockedTime (reason, duration)
- **Invariants**:
  - Availability blocks cannot overlap
  - Blocked time takes precedence over availability
  - Schedule changes do not affect already-confirmed appointments

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| AppointmentScheduled | New booking created | Patient (notification), Clinical |
| AppointmentConfirmed | Patient confirms | Clinical |
| AppointmentCancelled | Booking cancelled | Patient (notification), Waitlist processing |
| AppointmentRescheduled | Time or provider changed | Patient (notification), Clinical |
| AppointmentCheckedIn | Patient arrives | Clinical (creates encounter) |
| AppointmentNoShow | Patient did not arrive | Patient (follow-up), Waitlist processing |
| ProviderScheduleUpdated | Availability changes | Appointment conflict review |

## Domain Services

### AppointmentConflictChecker

Verifies a proposed appointment does not conflict with existing bookings for the same provider. Accounts for appointment duration, buffer time, and provider-specific rules.

### WaitlistProcessor

When an appointment is cancelled, automatically offers the slot to the highest-priority patient on the waitlist for that provider or procedure type.

### RecurringAppointmentGenerator

Creates a series of future appointments based on a recurrence pattern (e.g., weekly physical therapy for 8 weeks).

## Integration with Other Contexts

### Scheduling → Clinical/EMR (Customer-Supplier)

When a patient checks in (AppointmentCheckedIn), the Clinical context creates an outpatient encounter. The Clinical context is the downstream consumer.

### Patient → Scheduling (Customer-Supplier)

Scheduling consumes patient identity (MRN, name, contact info) from the Patient context to associate bookings with patients and send reminders.

### Scheduling → Billing (Indirect)

Scheduling does not directly communicate with Billing. Instead, the Clinical context's encounter (triggered by check-in) eventually flows to Billing. However, missed appointments (NoShow) may trigger administrative fees through a separate process.

## Key Workflows

### Book Appointment

1. Patient or staff selects provider and desired date/time
2. AppointmentConflictChecker verifies availability
3. Appointment aggregate created with status Scheduled
4. AppointmentScheduled event published
5. Patient receives confirmation notification

### Cancel and Rebook from Waitlist

1. Patient cancels appointment
2. AppointmentCancelled event published
3. WaitlistProcessor identifies eligible waitlisted patients
4. Highest-priority patient is offered the slot
5. If accepted, new AppointmentScheduled event is published

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of five hospital bounded contexts
- [Context Map](../context-map/) — Customer-supplier relationship with Clinical context
- [Aggregates](../aggregates/) — Appointment and Schedule are aggregate roots
- [Domain Events](../domain-events/) — Scheduling events trigger clinical encounter creation
- [Domain Services](../domain-services/) — Conflict checking and waitlist processing are domain services

## References

- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 12. Wrox, 2015.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- HL7 International. "FHIR R4 Appointment Resource." https://hl7.org/fhir/appointment.html
