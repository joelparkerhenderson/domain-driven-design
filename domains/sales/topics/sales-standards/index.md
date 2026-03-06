# Sales Standards in the Sales Domain

## Overview

Sales standards encompass the industry standards, data interchange formats, methodologies,
and regulatory frameworks that inform the design of the Sales domain model. In Domain-Driven
Design, standards shape value object definitions, published languages, and integration
interfaces. The Sales domain interacts with numerous standards ranging from currency codes
and country identifiers to CRM data models, sales methodology frameworks, and electronic
data interchange formats. Adopting relevant standards ensures interoperability, data quality,
and professional credibility.

Standards in the Sales domain serve as a published language that facilitates communication
both within the organization and with external partners, customers, and systems. When the
domain model aligns with industry standards, integration becomes simpler and the ubiquitous
language gains precision.

## Key Concepts

**ISO Standards for Data Representation**:

- ISO 4217: Currency codes (USD, EUR, GBP) used in the Money and Price value objects
  throughout the Sales domain.

- ISO 3166: Country codes used in Address value objects for billing and shipping
  address standardization.

- ISO 8601: Date and time formats used in DateRange value objects, contract terms, and
  event timestamps across all bounded contexts.

- ISO 20022: Financial messaging standards relevant to payment processing in the Order
  Management Context and cross-border transactions.

**CRM Data Standards**:

- OASIS Customer Information Quality (CIQ): Standards for representing customer name,
  address, and contact information consistently across systems.

- Common Data Model (CDM): Shared data schemas for business entities like Account,
  Contact, Opportunity, and Lead, promoted by major CRM platforms to enable
  interoperability.

**Sales Methodology Frameworks**:

- BANT (Budget, Authority, Need, Timeline): A qualification framework used in the Lead
  Generation Context to assess lead readiness for sales engagement.

- MEDDIC (Metrics, Economic Buyer, Decision Criteria, Decision Process, Identify Pain,
  Champion): An advanced qualification methodology used in the Opportunity Management
  Context for complex enterprise deals.

- Sandler Selling System: A methodology emphasizing mutual qualification and structured
  selling processes with defined rules of engagement.

- SPIN Selling (Situation, Problem, Implication, Need-Payoff): A questioning framework
  for consultative sales that structures discovery conversations.

- Challenger Sale: A methodology based on teaching, tailoring, and taking control of
  the sales conversation, particularly effective in complex B2B sales.

**Data Interchange Formats**:

- EDI (Electronic Data Interchange): Standards for business document exchange, relevant
  to order transmission in the Order Management Context.

- JSON Schema: Used for defining event schemas in the published language between
  bounded contexts and for API contracts in open host services.

**Revenue Recognition Standards**:

- ASC 606 (US) and IFRS 15 (International): Revenue recognition standards that influence
  how the Order Management and Account Management Contexts record revenue from contracts.
  These standards govern the timing and amount of revenue recognition.

## Sales Domain Application

Standards inform the Sales domain model in several ways. The Money value object validates
currency codes against ISO 4217. The Address value object uses ISO 3166 country codes. Event
timestamps follow ISO 8601. Lead qualification uses BANT or MEDDIC criteria encoded in the
LeadQualificationService. Revenue recognition rules from ASC 606 influence how contract
values are calculated and recognized in the Account Management Context.

## Relationship to Other Topics

- [Value Objects](../value-objects/) implement standards through validated attributes.
- [Ubiquitous Language](../ubiquitous-language/) incorporates standard terminology.
- [Integration Patterns](../integration-patterns/) use standard interchange formats.
- [Regulatory Compliance](../regulatory-compliance/) intersects with standards where compliance mandates formats.
- [Anti-Corruption Layer](../anti-corruption-layer/) translates between internal and external standards.
- [Lead Generation Context](../lead-generation-context/) uses methodology standards for qualification.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
- International Organization for Standardization. "ISO 4217: Currency Codes." ISO, 2015.
- Rackham, Neil. "SPIN Selling." McGraw-Hill, 1988.
- FASB. "ASC 606: Revenue from Contracts with Customers." 2014.
