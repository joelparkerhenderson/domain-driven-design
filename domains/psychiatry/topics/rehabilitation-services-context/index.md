# Rehabilitation Services Context

## Overview

The Rehabilitation Services Context is a supporting bounded context in the psychiatry domain responsible for managing psychosocial rehabilitation interventions. This context owns the workflows for developing individualized rehabilitation plans, coordinating supported employment, arranging housing assistance, delivering social skills training and cognitive remediation, and tracking functional milestones toward community reintegration. It maintains a partnership relationship with the Psychotherapy Context and publishes milestone events to the Outcomes Measurement Context.

## Responsibility

This context answers the clinical question: "How can this patient rebuild functional capacity and participate meaningfully in community life?" It encompasses the recovery-oriented services that address the functional impairments associated with serious mental illness, extending beyond symptom management to focus on social, vocational, and independent living outcomes. The context does not treat symptoms directly; it addresses the functional consequences of psychiatric conditions.

## Ubiquitous Language

Within this context, the following terms have specific meanings:

- **Rehabilitation Plan**: An individualized, goal-oriented document specifying the rehabilitation services, providers, milestones, and timelines for a patient's recovery journey.
- **Psychosocial Rehabilitation**: Structured interventions aimed at restoring social functioning, daily living skills, and community participation for individuals with serious mental illness.
- **Supported Employment**: A vocational rehabilitation approach following the Individual Placement and Support (IPS) model, helping individuals obtain and maintain competitive employment in integrated community settings.
- **Cognitive Remediation**: Behavioral training interventions targeting neurocognitive deficits (attention, working memory, executive function) commonly associated with schizophrenia, bipolar disorder, and other serious mental illnesses.
- **Functional Milestone**: A measurable achievement in a specific functional domain (vocational, social, residential, cognitive) that demonstrates progress toward rehabilitation goals.
- **Community Integration**: The process and outcome of a patient participating in community life through employment, social relationships, housing, and civic engagement.
- **Recovery**: A personal process of growth and community participation that may occur alongside ongoing symptom management; not defined solely by symptom remission.

## Aggregates

### Rehabilitation Plan (Root Aggregate)

The central aggregate representing a patient's comprehensive rehabilitation program.

Components:
- Patient reference and primary diagnosis reference
- Rehabilitation goals organized by functional domain (vocational, social, residential, cognitive, self-care)
- Service categories and assigned service providers
- Milestone definitions with target dates and assessment criteria
- Plan start date and estimated duration
- Review schedule (minimum every six months)
- Assigned case manager reference
- Plan status (draft, active, under review, completed, discontinued)
- Plan revision history

Invariants:
- A rehabilitation plan must be reviewed at least every six months.
- Each rehabilitation goal must have at least one associated measurable milestone.
- A plan cannot be marked as completed unless all primary milestones are either achieved or formally revised.
- The plan must specify at least one service category with an assigned provider.

### Functional Milestone (Root Aggregate)

Represents a specific measurable achievement within a rehabilitation plan.

Components:
- Milestone description aligned with rehabilitation goal
- Functional domain (vocational, social, residential, cognitive, self-care)
- Target date
- Assessment criteria (how achievement will be measured)
- Evidence of achievement (when completed)
- Current status (not started, in progress, achieved, revised, not achieved)
- Barriers encountered and strategies applied
- Supporting assessment data (functional assessment scores)

Invariants:
- A milestone cannot be marked as achieved without documented evidence.
- Target dates must fall within the associated rehabilitation plan period.
- Revised milestones must document the reason for revision and the new target.

## Value Objects

- **Functional Score**: Assessment instrument, functional domain, score, interpretation.
- **Service Category**: Category name (supported employment, housing support, skills training, cognitive remediation, peer support), description, evidence base.
- **Employment Outcome**: Employment type (competitive, supported, transitional), hours per week, wage, job tenure, workplace accommodations.
- **Housing Status**: Housing type (independent, supported, transitional, supervised residential, homeless), stability duration, support level.
- **Skill Level**: Skill domain, baseline level, current level, assessment method.

## Domain Events

- **Rehabilitation Plan Created**: New rehabilitation plan established for a patient.
- **Rehabilitation Plan Reviewed**: Scheduled review completed with updates documented.
- **Milestone Achieved**: Patient achieves a defined functional milestone.
- **Milestone Revised**: Milestone target modified due to changing circumstances.
- **Employment Placement Made**: Patient placed in a competitive employment position.
- **Housing Secured**: Patient obtains stable housing arrangement.
- **Cognitive Remediation Course Completed**: Patient completes a structured cognitive training program.

## Domain Services

- **Service Matching Service**: Matches patient rehabilitation needs with available community service providers based on location, availability, and specialization.
- **Functional Progress Assessment Service**: Evaluates a patient's functional trajectory across multiple domains using standardized functional assessment instruments.

## Service Models

### Individual Placement and Support (IPS)

The evidence-based model for supported employment that the context implements:
- Zero exclusion: All patients who express interest are eligible.
- Rapid job search: Employment search begins within 30 days.
- Competitive employment: Focus on jobs in integrated settings at market wages.
- Integration with mental health treatment: Employment specialists are part of the treatment team.
- Patient preferences drive job selection.
- Ongoing support without time limits.

### Housing First

The evidence-based approach to housing assistance:
- Immediate access to permanent housing without treatment prerequisites.
- Housing is separated from treatment compliance requirements.
- Support services are offered but not mandated.
- Focus on housing stability as a platform for recovery.

### Cognitive Remediation Programs

Structured cognitive training following established protocols:
- Computer-based cognitive exercises targeting specific domains.
- Therapist-guided bridging sessions connecting cognitive gains to real-world functioning.
- Integration with vocational and social rehabilitation goals.
- Typical duration of 24-40 sessions over 12-16 weeks.

## Integration Points

- **Partnership with Psychotherapy Context**: Coordinates treatment goals for patients receiving concurrent therapy and rehabilitation.
- **Downstream from Diagnostic Assessment**: Receives diagnostic information to inform rehabilitation planning.
- **Upstream to Outcomes Measurement**: Publishes milestone and functional progress events for outcome tracking.
- **Downstream from Crisis Management**: Receives crisis resolution events that may prompt rehabilitation plan revision.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Bond, Gary R., et al. "An Update on Individual Placement and Support." *World Psychiatry*, 2020.
- Wykes, Til, et al. "A Meta-analysis of Cognitive Remediation for Schizophrenia." *American Journal of Psychiatry*, 2011.
- Tsemberis, Sam. *Housing First: The Pathways Model to End Homelessness*. Hazelden, 2010.
