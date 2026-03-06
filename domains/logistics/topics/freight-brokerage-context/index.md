# Freight Brokerage Context in Logistics

## Overview

The Freight Brokerage Context is the bounded context responsible for managing the commercial relationship between shippers (who have freight to move) and carriers (who have capacity to move it). It encompasses load posting and discovery, carrier matching, rate negotiation, tender management, load board operations, carrier performance tracking, and freight audit. This context is the revenue engine of a brokerage-based logistics operation.

## Relevance to Logistics

Freight brokerage is a $250+ billion industry in the United States alone. The core challenge is matching available freight with available truck capacity at a price that works for both shipper and carrier while preserving broker margin. This context must handle volatile spot market pricing, manage thousands of carrier relationships, integrate with external load boards, and make rapid decisions about which carrier to tender each load to. The intelligence of carrier matching and rate negotiation directly determines profitability.

## Bounded Context Boundaries

**Owns**: LoadPosting aggregate, FreightQuote aggregate, BrokerContract aggregate, carrier performance data, rate negotiation logic, load board integration

**Does not own**: Vehicle and driver details (Fleet Management Context), route planning (Route Optimization Context), customs documentation (Customs & Clearance Context), physical delivery (Last-Mile Delivery Context)

**Upstream contexts**: Shippers and customers (provide freight to be moved)

**Downstream contexts**: Route Optimization Context (receives accepted loads for routing), Fleet Management Context (queries capacity), Customs & Clearance Context (provides international shipment details)

## Core Capabilities

### Load Management

- **Load creation**: Capturing shipment details from shippers including origin, destination, commodity, weight, dimensions, equipment type, pickup date, and delivery date
- **Load posting**: Publishing available loads to internal systems and external load boards for carrier visibility
- **Load matching**: Automatically identifying carriers with capacity on the required lane and equipment type
- **Load tracking**: Monitoring load status through the lifecycle from posted to tendered to in-transit to delivered

### Rate Management

- **Spot rate negotiation**: Negotiating one-time rates for individual loads based on current market conditions, lane supply/demand, and carrier availability
- **Contract rate management**: Managing pre-negotiated rates with carriers for committed lanes and volumes, typically agreed quarterly or annually
- **Rate benchmarking**: Comparing offered rates against market indices (DAT, Truckstop, Freightwaves SONAR) to ensure competitiveness
- **Accessorial calculation**: Computing additional charges for detention, layover, lumper fees, liftgate, inside delivery, and other non-standard services
- **Fuel surcharge application**: Applying fuel surcharge tables based on current DOE diesel price index

### Carrier Management

- **Carrier onboarding**: Verifying carrier authority (MC number), insurance coverage, safety rating (CSA scores), and operational capabilities before approving for load tendering
- **Carrier scoring**: Maintaining performance scorecards based on on-time pickup, on-time delivery, tender acceptance rate, claims ratio, and communication responsiveness
- **Carrier compliance**: Monitoring carrier insurance expiration, authority status changes, and safety rating downgrades
- **Preferred carrier programs**: Managing tiered carrier relationships with volume commitments and rate advantages

### Tender Management

- **Tender creation**: Formally offering a load to a carrier at a specified rate with acceptance deadline
- **Tender waterfall**: Automatically cascading a load tender through a ranked list of carriers until one accepts
- **Tender acceptance and rejection**: Processing carrier responses and updating load status accordingly
- **Rebrokering**: Re-tendering a load when the original carrier falls through after acceptance

### Load Board Integration

- **External posting**: Publishing loads to external load boards (DAT, Truckstop.com, Uber Freight) when internal carrier capacity is insufficient
- **External discovery**: Searching external load boards for backhaul opportunities to reduce empty miles
- **Rate intelligence**: Consuming market rate data from load boards and rate indices for pricing decisions

## Key Aggregates and Entities

- **LoadPosting** (aggregate root): A freight opportunity with origin, destination, commodity, equipment type, time constraints, and lifecycle status
- **FreightQuote** (entity): A carrier's quoted rate for a specific load, with expiration time and terms
- **BrokerContract** (aggregate root): A rate agreement between broker and carrier for specified lanes, volumes, and time periods
- **Carrier** (entity): A transportation provider with MC number, insurance, safety rating, and performance history
- **FreightRate** (value object): A monetary rate per unit (per mile, per hundredweight, flat) with fuel surcharge terms
- **Lane** (value object): An origin-destination pair defining a freight corridor
- **FreightClass** (value object): NMFC classification determining LTL pricing category

## Domain Events Produced

- **LoadPosted**: A new load has been published for carrier bidding
- **LoadTendered**: A load has been formally offered to a carrier
- **TenderAccepted**: A carrier has accepted a load tender
- **TenderRejected**: A carrier has declined a load tender
- **RateNegotiated**: A rate has been agreed for a load
- **CarrierInvoiceReceived**: A carrier has submitted an invoice for a completed load
- **LoadDelivered**: A load has been confirmed delivered by the carrier

## Domain Events Consumed

- **RoutePlanned** (from Route Optimization Context): Confirms the load has been incorporated into a route
- **VehicleDispatched** (from Fleet Management Context): Confirms a vehicle is en route for the load
- **CustomsCleared** (from Customs & Clearance Context): Confirms international freight can proceed

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Freight Brokerage is one of the six core bounded contexts
- [Route Optimization Context](../route-optimization-context/) -- Receives accepted loads for route planning
- [Fleet Management Context](../fleet-management-context/) -- Queries capacity and receives dispatch confirmation
- [Customs & Clearance Context](../customs-clearance-context/) -- Partnership for international shipments
- [Domain Services](../domain-services/) -- CarrierMatchingService and RateCalculationService operate here
- [Logistics Standards](../logistics-standards/) -- EDI 204/210/214 standards govern carrier communication

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 13: Integrating Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 6: Bounded Contexts. O'Reilly, 2021.
- Transportation Intermediaries Association. "Broker/Carrier Agreement Guidelines." TIA, current edition.
- Federal Motor Carrier Safety Administration. "Broker Authority Requirements." 49 CFR Part 371. US Department of Transportation.
