# Subdomains in Enterprise Resource Planning

## Overview

Subdomains classify the natural divisions of a business domain by strategic importance. In ERP, this classification guides where to invest the deepest modeling effort and where to use off-the-shelf solutions.

## Subdomain Classification

### Core Domain

The functions that provide competitive advantage. Varies by organization type:

- **Manufacturing company**: Production planning, supply chain optimization, quality management
- **Distribution company**: Order fulfillment, logistics optimization, inventory management
- **Service company**: Resource scheduling, project costing, service delivery

**Investment strategy**: Custom-built, deeply modeled, staffed with senior engineers and close business expert collaboration.

### Supporting Subdomains

Necessary for the business but not the primary differentiator:

- **Financial Accounting**: General ledger, AP/AR, financial reporting — important but follows standard accounting rules (GAAP/IFRS)
- **Human Resources**: Employee records, payroll, benefits — essential but well-understood processes
- **Basic Procurement**: Purchase orders, supplier management — needed but follows standard procurement workflows
- **Basic Inventory**: Stock tracking, warehouse management — required but not unique to the organization

**Investment strategy**: Well-modeled but may follow established industry patterns. Consider packaged solutions with customization.

### Generic Subdomains

Commoditized capabilities not specific to ERP:

- **Authentication and Authorization**: User login, role-based access, SSO
- **Email and Notifications**: Alerts, reminders, system messages
- **Document Management**: File storage, versioning, attachments
- **Reporting Infrastructure**: Report generation, dashboards, data export
- **Workflow Engine**: Approval chains, routing rules

**Investment strategy**: Buy or adopt open-source. Do not build custom.

## Subdomain Map

| Subdomain | Classification | Bounded Context | Priority |
|-----------|---------------|----------------|----------|
| Supply Chain Optimization | Core | Procurement + Inventory | Highest |
| Production Planning | Core | Manufacturing | Highest |
| Order Management | Core | Sales/Order | Highest |
| Financial Accounting | Supporting | Finance/Accounting | Medium |
| Payroll & HR Admin | Supporting | Human Resources | Medium |
| Basic Warehousing | Supporting | Inventory/Warehouse | Medium |
| Authentication | Generic | External | Low |
| Email/Notifications | Generic | External | Low |

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) — Subdomain identification is a strategic design activity
- [Bounded Contexts](../bounded-contexts/) — Subdomains map to bounded contexts
- [Context Map](../context-map/) — Shows relationships between subdomain-aligned contexts

## References

- Evans, Eric. "Domain-Driven Design." Chapter 15. Addison-Wesley, 2003.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2. Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 3. Wrox, 2015.
