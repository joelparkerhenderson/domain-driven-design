# Project Portfolio Management (PPM) Domain Driven Design (DDD)

Driven Design (DDD) to Project Portfolio Management (PPM) aligns complex business processes like project selection, resource allocation, and risk management with a structured software model. This approach focuses on creating a Ubiquitous Language shared by project managers and developers to ensure the software accurately reflects real-world portfolio operations. 

## Strategic PPM Domains

DDD divides the PPM landscape into subdomains based on their strategic importance to the organisation: 

Core Domain: The primary focus that provides a competitive edge, such as a proprietary Project Selection and Prioritisation algorithm or Strategic Alignment engine.

Supporting Domain: Essential areas that support the core but are not unique, like Resource Capacity Planning or Financial Tracking.

Generic Domain: Standard functionalities that can be satisfied by off-the-shelf solutions, such as User Management, Email Notifications, or Document Storage. 

## PPM Bounded Contexts

To manage complexity, PPM is split into Bounded Contexts, which are logical boundaries where specific models and terminologies are consistent. Potential contexts include: 

Portfolio Governance: Focuses on high-level oversight, investment categories, and KPIs.

Execution Management: Deals with individual project progress, milestones, and task-level data.

Risk Management: Concentrates on identifying, quantifying, and mitigating portfolio-wide risks. 

## Core PPM Tactical Patterns

In implementation, tactical patterns define how the business logic is structured.

Entities: Objects with a unique identity, such as a Project or Portfolio, which persist over time even as their attributes change.

Value Objects: Objects defined by their attributes rather than identity, like a CurrencyAmount or DateRange used for budget or scheduling.

Aggregates: Clusters of related objects treated as a single unit, such as a ProjectPlan aggregate that maintains consistency across its tasks and resources.

Domain Events: Significant occurrences within the domain, such as ProjectApproved or BudgetExceeded, which can trigger actions in other bounded contexts. 
