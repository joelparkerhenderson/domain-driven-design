# Anti-Corruption Layer in the Sales Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from
the concepts, terminology, and models of external systems. In the Sales domain, anti-corruption
layers are essential when integrating with external CRM platforms, marketing automation tools,
ERP systems, and third-party data providers. The ACL translates between the foreign model and
the Sales domain's own ubiquitous language, ensuring that external system complexity does not
pollute the internal domain model.

Sales organizations frequently integrate with multiple external systems: marketing platforms
like HubSpot or Marketo for lead generation, CRM systems like Salesforce for customer data,
ERP systems like SAP for order processing, and analytics platforms for business intelligence.
Each of these systems has its own data model and terminology that differs from the Sales
domain's ubiquitous language.

## Key Concepts

**Translation Layer**: The ACL acts as a translator between two models. When an external CRM
system uses the term "Contact" to mean any person in the database, the ACL translates this
into the Sales domain's specific concepts of Lead, Prospect, or Contact depending on the
qualification status and context.

**Model Protection**: Without an ACL, external system concepts leak into the domain model,
creating confusion and coupling. For example, if the Sales domain directly consumes a
marketing platform's "MQL" (Marketing Qualified Lead) object, changes to the marketing
platform's definition of MQL would ripple through the Sales model.

**Bidirectional Translation**: ACLs often handle translation in both directions. When the
Sales domain sends order data to an ERP system, the ACL translates Sales domain concepts
(Order, OrderLine, FulfillmentHandoff) into the ERP system's vocabulary (SalesOrder,
LineItem, ShipmentRequest).

**Isolation of Change**: The ACL isolates the domain model from changes in external systems.
When a third-party CRM vendor releases a new API version with different field names and
data structures, only the ACL needs to be updated, not the domain model itself.

## Sales Domain Applications

**CRM Integration ACL**: Translates between an external CRM platform's data model and the
Sales domain's Lead Generation and Account Management contexts. Maps external contact
records to the appropriate internal entities based on qualification status and relationship
type.

**Marketing Automation ACL**: Translates marketing campaign data and lead scoring signals
from external marketing platforms into the Lead Generation Context's model. Converts
platform-specific engagement metrics into the domain's LeadScore value object.

**ERP Integration ACL**: Translates closed deal data from the Order Management Context into
the format required by external ERP or fulfillment systems. Handles differences in product
catalogs, pricing structures, and order processing workflows.

**Data Enrichment ACL**: Translates third-party data enrichment feeds (firmographic data,
contact databases, intent signals) into the Sales domain's value objects and entity
attributes, filtering and normalizing the data during translation.

**Legacy System ACL**: When migrating from a legacy sales system to a new DDD-based
architecture, an ACL protects the new model from the legacy system's accumulated conceptual
debt while maintaining interoperability during the transition period.

## Relationship to Other Topics

- [Context Map](../context-map/) shows where ACLs are positioned between contexts and external systems.
- [Integration Patterns](../integration-patterns/) describes the broader set of patterns ACLs participate in.
- [Bounded Contexts](../bounded-contexts/) define the internal models that ACLs protect.
- [Ubiquitous Language](../ubiquitous-language/) provides the target vocabulary ACLs translate into.
- [Domain Events](../domain-events/) may be produced or consumed through ACL translations.
- [Value Objects](../value-objects/) are often the target type for ACL translations.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 14: Maintaining Model Integrity.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 3: Context Maps.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 5: Context Mapping.
- Hohpe, Gregor and Woolf, Bobby. "Enterprise Integration Patterns."
  Addison-Wesley, 2003.
