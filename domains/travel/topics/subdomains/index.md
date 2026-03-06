# Subdomains in Travel

## Overview

Subdomains classify areas of a domain by their strategic importance to the business. Domain-Driven Design identifies three types: core subdomains that provide competitive advantage, supporting subdomains that are necessary but not differentiating, and generic subdomains that are common across industries. This classification guides investment decisions -- where to build custom solutions, where to adapt existing patterns, and where to buy off-the-shelf.

## Relevance to Travel

Travel platforms compete on fare optimization, search speed, booking reliability, and customer experience. Not every component is equally strategic. A superior pricing engine or faster availability search directly impacts revenue and market position, while user authentication or email delivery does not differentiate one travel platform from another. Subdomain classification ensures engineering effort is concentrated where it matters most.

## Core Subdomains

Core subdomains are the areas where the travel platform creates unique value and competitive advantage. These deserve the most talented engineers, custom-built solutions, and continuous investment.

- **Fare Optimization and Pricing Engine**: Dynamic pricing, fare combination algorithms, revenue management rules, and discount strategies that maximize revenue per booking
- **Real-Time Availability Aggregation**: Efficiently querying multiple GDS platforms and direct connections, caching strategies for volatile inventory, ranking and filtering algorithms
- **Reservation Lifecycle Management**: The booking engine that creates, modifies, and cancels reservations with transactional integrity across multiple external systems
- **Search Ranking and Personalization**: Algorithms that rank and present search results based on relevance, passenger preferences, and business rules

## Supporting Subdomains

Supporting subdomains are necessary for the system to function but do not provide primary competitive differentiation. They may use established patterns or adapt existing solutions.

- **Passenger Profile Management**: Storing passenger identities, documents, preferences, and loyalty memberships
- **Itinerary Assembly**: Combining confirmed segments into presentable travel documents with connection validation
- **Ancillary Service Management**: Seat selection, baggage, meals, lounge access, and other add-on services
- **Notification and Communication**: Booking confirmations, schedule change alerts, check-in reminders
- **Reporting and Analytics**: Booking volumes, revenue reports, conversion metrics, operational dashboards

## Generic Subdomains

Generic subdomains are common across all industries and should use commodity solutions. Building custom implementations here wastes resources.

- **Authentication and Authorization**: User login, role-based access control, single sign-on
- **Email and SMS Delivery**: Message routing and delivery infrastructure
- **File Storage**: Document and receipt storage
- **Logging and Monitoring**: Application observability, error tracking, performance monitoring
- **Currency Conversion**: Exchange rate lookups using standard financial data feeds

## Subdomain Boundaries and Bounded Contexts

Each subdomain does not necessarily map one-to-one to a bounded context. A single bounded context may span multiple supporting subdomains if they share the same model and team. Conversely, a core subdomain like reservation management may warrant its own bounded context with dedicated team ownership to ensure focused investment and rapid iteration.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain classification is a core strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains inform bounded context boundaries
- [Context Map](../context-map/) -- Subdomain types influence relationship patterns on the context map
- [Domain Services](../domain-services/) -- Core subdomain logic often resides in domain services
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Generic subdomains frequently sit behind ACLs

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 3: Managing Domain Complexity. O'Reilly, 2021.
