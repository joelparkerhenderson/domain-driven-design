# Legal Sector Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to legal sector systems, encompassing case management, client relationship management, document and contract lifecycle, legal billing and trust accounting, regulatory compliance, and litigation support.

## Bounded Contexts

1. Case Management Context - matter intake, case lifecycle tracking, deadline and statute management, court filing coordination, case outcome recording
2. Client Relationship Context - client onboarding, conflict of interest checking, engagement letter management, client communication tracking, relationship history
3. Document and Contract Lifecycle Context - document drafting and assembly, version control, contract negotiation tracking, clause library management, electronic signature workflows
4. Legal Billing and Trust Accounting Context - time entry recording, fee arrangement management (hourly, contingency, flat fee), invoice generation, trust account (IOLTA) management, payment processing
5. Regulatory Compliance Context - ethics rule enforcement, bar admission tracking, continuing legal education (CLE) compliance, data privacy (attorney-client privilege), anti-money laundering screening
6. Litigation Support Context - discovery management, evidence cataloging, deposition scheduling, expert witness coordination, trial preparation and exhibit management

## Domain Principles

- Model using shared ubiquitous language between attorneys, paralegals, legal administrators, compliance officers, and system developers.
- Group the system into bounded contexts reflecting legal practice operational areas and professional responsibility requirements.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow ABA Model Rules of Professional Conduct and jurisdiction-specific court rules for terminology and workflows.
- Maintain attorney-client privilege, data confidentiality, and ethical compliance as cross-cutting concerns.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- case-management-context
- client-relationship-context
- document-contract-lifecycle-context
- legal-billing-trust-accounting-context
- regulatory-compliance-context
- litigation-support-context
- event-driven-architecture
- integration-patterns
- legal-standards
- ethical-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- American Bar Association. Model Rules of Professional Conduct. ABA Publishing, 2020.
- Susskind, Richard. Tomorrow's Lawyers: An Introduction to Your Future. Oxford University Press, 2017.
