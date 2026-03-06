# Monitoring & Alerts Context

The Monitoring and Alerts Context is the bounded context responsible for real-time surveillance of cardiac metrics, threshold-based alerting, emergency notification workflows, and escalation management. It continuously evaluates incoming cardiac measurements and analysis findings against configurable rules to ensure that clinically significant events receive timely attention.

## Overview

The Monitoring and Alerts Context operates as a real-time event processing system within the heart health metrics domain. It subscribes to domain events from the Data Collection and Cardiac Analysis contexts, evaluates each event against patient-specific alert rules, and triggers appropriate notifications when thresholds are breached. This context is the domain's safety net, ensuring that dangerous cardiac conditions such as sustained ventricular tachycardia, hypertensive crisis, or prolonged asystole are detected and communicated without delay.

This context is classified as a supporting subdomain. While it is essential for clinical safety, its core logic (rule evaluation, threshold comparison, notification routing) follows well-established patterns. The differentiating clinical intelligence resides in the Cardiac Analysis Context; the Monitoring and Alerts Context applies that intelligence to real-time surveillance.

## Key Concepts

**Alert Rules.** Configurable rules that define the conditions under which an alert is triggered. Each rule specifies the metric being monitored (heart rate, blood pressure, arrhythmia type), the threshold values (e.g., heart rate above 150 bpm), the comparison logic (greater than, less than, sustained for duration), and the severity level (informational, warning, critical, emergency).

**Threshold Configuration.** Patient-specific or population-default threshold settings for cardiac metrics. Thresholds account for individual variability: an athlete with a resting heart rate of 48 bpm should not trigger a bradycardia alert at the same threshold as a sedentary elderly patient. The Alert Subscription aggregate manages threshold configurations.

**Notification Channels.** The mechanisms through which alerts are delivered: in-app notifications, SMS, phone calls, pager messages, email, or EHR inbox messages. Each alert subscription specifies preferred channels for different severity levels. Critical alerts may use multiple channels simultaneously.

**Escalation Policies.** Rules governing what happens when an alert is not acknowledged within a defined time period. Escalation may involve notifying a secondary clinician, contacting the on-call cardiologist, or activating an emergency response protocol. Escalation chains prevent alerts from being missed during shift changes or high-volume periods.

**Alert Lifecycle.** Alerts progress through states: triggered, delivered, acknowledged, resolved, or expired. The context tracks this lifecycle for audit purposes and to prevent alert fatigue from repeated notifications for the same ongoing condition.

**Alert Fatigue Mitigation.** The context implements strategies to reduce alert fatigue: suppression of duplicate alerts for ongoing conditions, aggregation of related alerts into summary notifications, and configurable quiet periods for non-critical alerts during nighttime hours.

## Domain Examples

A patient's continuous heart rate monitor reports a sustained heart rate of 165 bpm lasting more than 10 minutes. The Monitoring and Alerts Context evaluates this against the patient's alert rules, which have a critical threshold at heart rate above 150 bpm sustained for 5 minutes. The context triggers a critical alert, delivers it via SMS and pager to the patient's cardiologist, and starts a 15-minute escalation timer. If not acknowledged, the alert escalates to the on-call cardiac care team.

A patient's home blood pressure reading shows 195/118 mmHg. The context recognizes this as a hypertensive urgency (systolic above 180 or diastolic above 110) and triggers an emergency alert with instructions for the patient to seek immediate medical attention while simultaneously notifying the patient's primary care provider.

## Relationships to Other Topics

- [Cardiac Analysis Context](../cardiac-analysis-context/index.md) - Upstream provider of anomaly findings.
- [Data Collection Context](../data-collection-context/index.md) - Upstream provider of raw measurement events.
- [Domain Events](../domain-events/index.md) - Alert triggers are driven by domain events.
- [Aggregates](../aggregates/index.md) - Alert Subscription is a key aggregate.
- [Clinical Integration Context](../clinical-integration-context/index.md) - Alerts may be transmitted to EHR systems.
- [Event-Driven Architecture](../event-driven-architecture/index.md) - Monitoring relies on event-driven communication.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- Sendelbach, S. and Funk, M. "Alarm Fatigue: A Patient Safety Concern." *AACN Advanced Critical Care*, 24(4), 378-386, 2013.
- Drew, B.J. et al. "Practice Standards for Electrocardiographic Monitoring in Hospital Settings." *Circulation*, 110(17), 2721-2746, 2004.
- Topol, Eric. *The Patient Will See You Now*. Basic Books, 2015.
