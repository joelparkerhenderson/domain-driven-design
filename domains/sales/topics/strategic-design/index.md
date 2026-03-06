# Strategic Design in the Sales Domain

## Overview

Strategic design in Domain-Driven Design addresses the high-level decomposition of a complex
domain into manageable parts. For the Sales domain, strategic design determines how CRM and
sales operations are divided into bounded contexts, how those contexts relate to each other,
and which areas represent the greatest competitive advantage. Strategic design decisions shape
the architecture of the entire Sales system and directly influence team organization,
integration patterns, and long-term maintainability.

The Sales domain is particularly well-suited to strategic design analysis because it encompasses
diverse business capabilities ranging from marketing-driven lead generation to legally-binding
contract management. Each of these areas has distinct terminology, business rules, and
stakeholders, making careful boundary definition essential.

## Key Concepts

**Domain Decomposition**: The Sales domain is decomposed into six bounded contexts: Lead
Generation, Opportunity Management, Sales Pipeline, Order Management, Account Management, and
Sales Analytics. Each context encapsulates a coherent set of business rules and maintains its
own model integrity.

**Core Domain Identification**: Opportunity Management represents the core subdomain where
competitive differentiation occurs. The negotiation process, proposal customization, and
deal-closing strategies are unique to each organization and directly drive revenue.

**Problem Space vs. Solution Space**: Strategic design distinguishes between the problem space
(the actual business challenges in sales operations) and the solution space (the software
models and bounded contexts designed to address them). Understanding the problem space requires
close collaboration with sales managers, representatives, and operations teams.

**Distillation**: The process of identifying the most essential aspects of the Sales domain.
The core sales logic around deal management and customer relationships must be distilled from
supporting concerns like reporting and generic concerns like order processing.

## Sales Domain Application

In the Sales domain, strategic design begins with understanding the end-to-end sales lifecycle.
A prospect enters through marketing efforts, becomes a qualified lead, transitions into an
opportunity within the pipeline, and ultimately converts into a confirmed order tied to a
customer account. Each transition crosses bounded context boundaries and requires careful
integration design.

Strategic design also addresses team alignment. Conway's Law suggests that system architecture
mirrors organizational structure. Sales teams are typically organized by function (marketing,
inside sales, field sales, account management), and bounded context boundaries often align
with these team boundaries.

The context map reveals that Opportunity Management sits at the center of the Sales domain,
with upstream dependencies on Lead Generation and downstream relationships to Order Management
and Account Management. Sales Analytics operates as a reporting context that consumes events
from all other contexts.

Event Storming workshops are a valuable tool for discovering bounded context boundaries in the
Sales domain. By mapping the domain events that occur throughout the sales lifecycle, teams
can identify natural clusters of related events that suggest context boundaries. For example,
events related to lead scoring and qualification naturally cluster together, distinct from
events related to deal negotiation and proposal management.

Strategic design is not a one-time activity. As the Sales domain evolves with new products,
markets, and competitive dynamics, the bounded context boundaries and subdomain classifications
should be revisited. A supporting subdomain like Lead Generation may become core if
AI-driven lead scoring becomes a primary differentiator.

## Relationship to Other Topics

- [Bounded Contexts](../bounded-contexts/) define the logical boundaries identified through strategic design.
- [Context Map](../context-map/) visualizes the relationships between bounded contexts.
- [Subdomains](../subdomains/) classify domain areas by strategic importance.
- [Ubiquitous Language](../ubiquitous-language/) ensures consistent terminology within each bounded context.
- [Anti-Corruption Layer](../anti-corruption-layer/) protects context boundaries during integration.
- [Integration Patterns](../integration-patterns/) define how bounded contexts communicate.

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software."
  Addison-Wesley, 2003. Part IV: Strategic Design.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
  Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly Media, 2021.
  Part II: Strategic Design.
- Brandolini, Alberto. "Introducing EventStorming." Leanpub, 2021.
- Tune, Nick. "Principles of Domain-Driven Design." Packt Publishing, 2023.
