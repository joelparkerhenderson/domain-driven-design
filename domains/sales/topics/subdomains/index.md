# Subdomains in the Sales Domain

## Overview

Subdomains classify areas of a domain by their strategic importance to the business. In
Domain-Driven Design, subdomains are divided into three categories: core, supporting, and
generic. For the Sales domain, this classification determines where to invest the most
design effort, where to build custom solutions, and where to adopt off-the-shelf or
standardized approaches. Correctly classifying subdomains ensures that the highest-value
areas receive the most sophisticated modeling while commodity functions are handled
efficiently.

The Sales domain spans activities from marketing-driven lead generation through deal closure
and post-sale account management. Not all of these activities are equally important to
competitive differentiation, and subdomain classification helps allocate resources
accordingly.

## Key Concepts

**Core Subdomain**: The area where the organization competes and differentiates itself. In
the Sales domain, Opportunity Management is the core subdomain. The specific ways a company
negotiates deals, structures proposals, manages competitive situations, and closes sales
represent its unique competitive advantage. This area warrants the most investment in custom
modeling and domain expertise.

**Supporting Subdomain**: Areas that are necessary for the core subdomain to function but do
not themselves represent competitive differentiation. In the Sales domain, the supporting
subdomains include:

- **Lead Generation**: Essential for feeding the pipeline but often follows industry-standard
  practices for campaign management and lead scoring.

- **Sales Pipeline**: Provides critical visibility into the sales process but uses
  well-understood stage-gate methodologies.

- **Account Management**: Maintains customer relationships and contracts but follows
  established CRM patterns for contact and contract management.

**Generic Subdomain**: Areas that are common across many businesses and can be addressed with
standard solutions. In the Sales domain, the generic subdomains include:

- **Order Management**: Order creation and processing follows well-established commercial
  patterns that do not vary significantly between organizations.

- **Sales Analytics**: Reporting and dashboarding are common business intelligence functions
  that can leverage standard tools and frameworks.

## Subdomain Classification Implications

Core subdomains receive bespoke domain modeling with deep investment in tactical design
patterns. The Opportunity Management Context should have the most refined aggregate design,
the most expressive domain events, and the most sophisticated business rules.

Supporting subdomains may use simpler models or established frameworks with targeted
customization. Lead Generation might adopt a proven scoring framework and customize only
the scoring weights and qualification criteria.

Generic subdomains can leverage off-the-shelf solutions, third-party services, or
standardized implementations. Order Management might use a standard order processing
framework, and Sales Analytics might build on an existing BI platform.

## Classification Evolution

This classification is not static. As market conditions change, a supporting subdomain like
Lead Generation might become core if AI-driven lead scoring becomes a key competitive
differentiator. Similarly, if the organization competes primarily on customer retention,
Account Management could shift from supporting to core. Regular reassessment ensures
alignment between subdomain classification and business strategy.

## Relationship to Other Topics

- [Strategic Design](../strategic-design/) uses subdomain classification as a key input.
- [Bounded Contexts](../bounded-contexts/) often align with subdomain boundaries.
- [Context Map](../context-map/) reflects dependencies between subdomains.
- [Domain Services](../domain-services/) may handle logic spanning subdomains.
- [Aggregates](../aggregates/) receive design investment proportional to subdomain importance.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Chapter 15: Distillation.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Chapter 3: Managing Domain Complexity.
- Tune, Nick. "Domain-Driven Architecture." InfoQ, 2019.
