# Strategic Design in Enterprise Resource Planning

## Overview

Strategic design in Domain-Driven Design (DDD) addresses how to decompose a large ERP system into manageable bounded contexts that align with business functions. ERP systems are notoriously complex — spanning finance, procurement, manufacturing, sales, inventory, and human resources. Strategic design provides the framework to manage this complexity without creating a monolithic system where every module depends on every other.

## Relevance to ERP

ERP systems integrate nearly every function of an organization. Without strategic design:

- A single "Order" model accumulates attributes from sales, procurement, manufacturing, and finance
- Changes in one module cascade unpredictably across the entire system
- Development teams step on each other's code constantly
- The system becomes impossible to test, deploy, or evolve independently

Strategic design decomposes the ERP into bounded contexts — each with its own model, language, and team ownership.

## Key Concepts

### Knowledge Crunching

Collaborative extraction of domain knowledge from business experts:

- Workshops with accountants to understand journal entry rules and financial close processes
- Sessions with warehouse managers to understand pick-pack-ship workflows
- Interviews with procurement specialists to understand sourcing and supplier evaluation
- Observation of manufacturing floor operations to understand production scheduling

### Domain Vision Statement

A concise statement capturing the ERP system's core purpose:

> "To provide an integrated platform that automates and coordinates core business operations — from procurement through production to customer delivery — using domain models that reflect how each department actually works, enabling operational efficiency while maintaining clear boundaries between functional areas."

### Core Domain Identification

- **Core Domain**: The functions that differentiate the organization — typically supply chain optimization, production planning, or customer order management
- **Supporting Subdomains**: Necessary but not differentiating — financial accounting, HR administration, basic inventory tracking
- **Generic Subdomains**: Commoditized capabilities — authentication, email, document storage

## ERP Domain Decomposition

Strategic design guides the ERP system into six bounded contexts:

- **Finance/Accounting** — General ledger, AP/AR, financial reporting, tax management
- **Procurement** — Purchase orders, supplier management, sourcing, receiving
- **Inventory/Warehouse** — Stock levels, warehouse operations, transfers, logistics
- **Sales/Order** — Sales orders, customer management, pricing, fulfillment
- **Human Resources** — Employee records, payroll, benefits, recruitment
- **Manufacturing** — Production planning, BOM, work orders, quality control

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — The primary output of strategic design
- [Context Map](../context-map/) — Visualizes relationships between bounded contexts
- [Subdomains](../subdomains/) — Classification by strategic importance
- [Ubiquitous Language](../ubiquitous-language/) — Shared vocabulary within each context
- [Anti-Corruption Layer](../anti-corruption-layer/) — Protects context boundaries

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapters 14–16. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Part I. Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Monk, Ellen & Wagner, Bret. "Concepts in Enterprise Resource Planning." Cengage Learning, 2012.
