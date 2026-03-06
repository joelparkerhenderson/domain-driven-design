# Subdomains in Energy

## Overview

Subdomains classify areas of a domain by their strategic importance and the level of investment they warrant. Domain-Driven Design identifies three types: core subdomains that provide competitive advantage and require custom solutions, supporting subdomains that are necessary but not differentiating, and generic subdomains that are commoditized and best served by off-the-shelf solutions. In the energy domain, this classification guides where to focus engineering talent and custom development.

## Relevance to Energy

Energy companies operate across a wide spectrum of activities, from real-time grid operations to monthly billing to regulatory reporting. Not all of these deserve the same level of engineering investment. A utility's competitive advantage may lie in dispatch optimization and renewable forecasting, not in payroll processing or email delivery. Subdomain classification ensures that the most valuable engineering effort goes to the areas that matter most.

## Core Subdomains

Core subdomains are where the energy organization differentiates itself. These require custom, high-quality domain models and the best engineering talent.

- **Generation Dispatch Optimization**: Algorithms that determine which units to dispatch, at what output levels, to minimize cost while meeting demand and reliability constraints. This is the heart of generation operations.
- **Market Bidding and Risk Management**: Strategies for bidding into day-ahead and real-time markets, managing price risk exposure, and optimizing the generation portfolio's market position.
- **Renewable Generation Forecasting**: Prediction models that forecast solar and wind output based on weather data, historical patterns, and equipment conditions. Accuracy directly impacts market position and grid reliability.
- **Real-Time Grid Balancing**: The continuous adjustment of generation and load to maintain grid frequency and stability. Errors in this area can cause cascading failures.
- **Battery Storage Optimization**: Algorithms that determine when to charge and discharge battery systems based on market prices, grid conditions, and renewable generation forecasts.

## Supporting Subdomains

Supporting subdomains are necessary for operations but are not the primary source of competitive advantage. They may use established industry patterns and frameworks.

- **Meter Data Management**: Collection, validation, and storage of interval consumption data from smart meters. Important for accuracy but follows well-established utility industry patterns.
- **Billing Cycle Processing**: Calculation of customer charges based on consumption data and tariff schedules. Must be accurate but is not a differentiator.
- **Outage Management**: Detection, tracking, and coordination of outage restoration activities. Critical for service quality but follows standard utility workflows.
- **Contract Administration**: Management of energy purchase agreements, fuel contracts, and interconnection agreements. Requires domain knowledge but follows established legal and commercial patterns.
- **Demand Response Administration**: Enrollment, notification, and settlement of demand response program participants. Important for grid reliability but increasingly standardized.
- **Compliance Reporting**: Preparation and submission of regulatory compliance reports to FERC, NERC, and state regulators. Must be accurate but follows prescribed formats.

## Generic Subdomains

Generic subdomains are commoditized capabilities that should use off-the-shelf solutions rather than custom development.

- **Identity and Access Management**: User authentication, role-based access control, and single sign-on.
- **Notification Services**: Email, SMS, and push notification delivery for outage alerts, billing notices, and demand response events.
- **Document Management**: Storage and retrieval of regulatory filings, contracts, and compliance documentation.
- **Logging and Monitoring**: System observability, application logging, and infrastructure monitoring.
- **Geospatial Services**: Map rendering and spatial queries for asset location, outage visualization, and service territory management.

## Subdomain Classification Criteria

When classifying energy subdomains, consider:

- **Competitive advantage**: Does excellence in this area differentiate the organization?
- **Complexity**: Does this area involve complex business rules that require deep domain knowledge?
- **Change frequency**: How often do the rules and requirements in this area change?
- **Market availability**: Are there mature commercial solutions that adequately address this area?
- **Regulatory mandate**: Is this area prescribed by regulation, leaving little room for differentiation?

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain classification is a strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains often map to bounded contexts
- [Context Map](../context-map/) -- Subdomain classification informs integration strategy
- [Domain Services](../domain-services/) -- Core subdomain logic often lives in domain services
- [Aggregates](../aggregates/) -- Core subdomains deserve the most carefully designed aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 1: Analyzing Business Domains. O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2: Strategic Design with Bounded Contexts and the Ubiquitous Language. Addison-Wesley, 2016.
