# Subdomains in the Orthopaedic Domain

## Overview

Subdomains classify areas of the orthopaedic domain by their strategic importance.
Domain-Driven Design distinguishes between core, supporting, and generic subdomains
to guide where to invest the most design effort, where to use simpler models, and
where to adopt off-the-shelf solutions.

## Purpose

Not all parts of the orthopaedic domain are equally complex or strategically important.
Subdomain classification ensures that limited development resources are concentrated
on the areas that provide the greatest clinical value and competitive differentiation,
while commodity functions are handled efficiently.

## Core Subdomains

Core subdomains represent the primary clinical and strategic value of the orthopaedic
system. They require the most sophisticated models, the deepest domain expert
involvement, and the highest quality code.

### Surgical Planning

Surgical planning is core because it directly affects surgical outcomes. Accurate
implant selection, templating, and approach planning reduce operative complications.
The model must capture complex anatomical relationships, surgeon preferences, and
implant compatibility rules that are unique to orthopaedic practice.

### Joint Replacement

Joint replacement is core because arthroplasty registries, implant tracking, and
revision management represent long-term patient safety obligations. The model must
support decades of longitudinal tracking, complex implant component relationships,
and outcome analysis that drives clinical improvement.

### Fracture Management

Fracture management is core because accurate classification, reduction assessment,
and healing monitoring directly impact patient recovery. The AO/OTA classification
system introduces complex hierarchical categorization that the model must faithfully
represent.

## Supporting Subdomains

Supporting subdomains are necessary for the system to function but do not represent
primary strategic differentiators. They require solid models but not the same depth
of investment as core subdomains.

### Clinical Assessment

Clinical assessment supports the core subdomains by providing diagnostic input.
While essential, the assessment workflow is relatively standardized across orthopaedic
practices. The model captures examination findings, imaging results, and provisional
diagnoses without the complexity of surgical or implant models.

### Sports Medicine

Sports medicine is supporting because, while clinically important, its workflows
(arthroscopy planning, return-to-play protocols) follow well-established clinical
guidelines. The model complexity is lower than surgical planning or fracture
classification.

### Rehabilitation

Rehabilitation supports recovery from surgical and non-surgical treatment. Protocol
selection and milestone tracking follow published clinical guidelines, and the model
is less complex than operative planning or implant management.

## Generic Subdomains

Generic subdomains are common to many healthcare systems and do not require custom
orthopaedic modeling. Off-the-shelf or standardized solutions are appropriate.

- Patient Identity Management: Matching and deduplicating patient records.
- Scheduling: Appointment and operating theatre scheduling.
- Billing and Coding: CPT/ICD coding and insurance claims.
- User Authentication and Authorization: Access control and role management.
- Notification: Email, SMS, and push notification delivery.

## Investment Strategy

| Subdomain            | Type       | Investment | Model Complexity |
|----------------------|------------|------------|------------------|
| Surgical Planning    | Core       | High       | High             |
| Joint Replacement    | Core       | High       | High             |
| Fracture Management  | Core       | High       | High             |
| Clinical Assessment  | Supporting | Medium     | Medium           |
| Sports Medicine      | Supporting | Medium     | Medium           |
| Rehabilitation       | Supporting | Medium     | Medium           |
| Scheduling           | Generic    | Low        | Low              |
| Billing              | Generic    | Low        | Low              |
| Identity Management  | Generic    | Low        | Low              |

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 1-3.
