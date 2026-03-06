# Subdomains in Logistics

## Overview

Subdomains classify areas of a domain by their strategic importance to the business. Domain-Driven Design identifies three types: core subdomains that provide competitive advantage, supporting subdomains that are necessary but not differentiating, and generic subdomains that are commoditized. In logistics, this classification determines where to invest custom modeling effort and where to use off-the-shelf solutions.

## Relevance to Logistics

A logistics company cannot afford to build everything custom. Route optimization algorithms that reduce fuel costs by 15% are a competitive advantage worth significant engineering investment. Driver compliance tracking is essential but follows well-established regulatory patterns. Email notification delivery is a commodity. Subdomain classification prevents over-engineering supporting functions and under-investing in core differentiators.

## Core Subdomains

Core subdomains are the areas where the logistics operation competes and differentiates. They deserve the best engineers, custom modeling, and continuous refinement.

- **Route Optimization**: Algorithms for multi-stop routing, load consolidation, real-time rerouting, and transit time minimization. These directly impact fuel cost, driver utilization, and delivery speed.
- **Carrier Matching and Rate Negotiation**: The logic that matches available freight with the best-fit carrier at the optimal price point. This drives margin in freight brokerage operations.
- **Fleet Dispatch and Utilization**: Decision logic for assigning the right vehicle and driver to each load, minimizing deadhead miles and maximizing asset utilization.
- **Last-Mile Delivery Optimization**: Stop sequencing, dynamic rerouting, and delivery window management that determine delivery efficiency and customer satisfaction.

## Supporting Subdomains

Supporting subdomains are necessary for the logistics operation to function but do not provide competitive differentiation. They follow well-known patterns and can use established solutions with moderate customization.

- **Driver Compliance and HOS Tracking**: Recording hours of service, managing ELD data, and ensuring regulatory compliance. The rules are defined by FMCSA and are the same for all carriers.
- **Customs Documentation**: Generating customs declarations, managing HS code classification, and filing with customs authorities. The forms and processes are standardized by government agencies.
- **Yard Inventory Management**: Tracking trailer positions, managing dock assignments, and recording gate transactions. The operational patterns are well-established across the industry.
- **Vehicle Maintenance Scheduling**: Preventive maintenance tracking, inspection scheduling, and repair history management. Standard fleet management practices apply.
- **Freight Audit and Payment**: Verifying carrier invoices against contracted rates and approving payment. This follows well-defined matching and exception-handling patterns.

## Generic Subdomains

Generic subdomains are commoditized capabilities available as off-the-shelf products or services. Build versus buy decisions should strongly favor buying or using open-source solutions.

- **User Authentication and Authorization**: Identity management, role-based access control, single sign-on
- **Email and SMS Notifications**: Delivery status notifications, appointment reminders, alert messages
- **File Storage and Document Management**: Storing BOL images, POD photos, customs documents
- **Reporting and Analytics Dashboards**: Standard operational reporting and KPI visualization
- **Logging and Monitoring**: Application logging, infrastructure monitoring, alerting

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain classification is a core strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains inform bounded context boundaries and investment levels
- [Route Optimization Context](../route-optimization-context/) -- A core subdomain
- [Freight Brokerage Context](../freight-brokerage-context/) -- Contains core subdomain logic for carrier matching
- [Fleet Management Context](../fleet-management-context/) -- Contains a mix of core (dispatch) and supporting (compliance) subdomains
- [Customs & Clearance Context](../customs-clearance-context/) -- Primarily a supporting subdomain

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 1: Analyzing Business Domains. O'Reilly, 2021.
- Tune, Nick & Millett, Scott. "Patterns, Principles, and Practices of Domain-Driven Design." Part I: Strategic Design. Wrox, 2015.
