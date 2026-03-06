# Regulatory & Grid Context in Energy

## Overview

The Regulatory & Grid Context is the bounded context responsible for ensuring compliance with federal and state energy regulations, maintaining grid reliability, administering demand response programs, providing ancillary services, and managing capacity obligations. It serves as the governance and reliability layer of the energy domain, translating regulatory mandates and grid reliability requirements into actionable obligations and programs.

## Relevance to Energy

The energy industry is one of the most heavily regulated sectors. Federal agencies (FERC), reliability organizations (NERC), and state public utility commissions impose detailed requirements on grid operation, market participation, environmental compliance, and customer service. The Regulatory & Grid Context centralizes the management of these obligations, preventing regulatory concerns from being scattered across generation, trading, and metering contexts where they would create inconsistencies and compliance gaps.

## Bounded Context Boundaries

**Owns**: ComplianceObligation aggregate, DemandResponseProgram aggregate, CapacityCommitment aggregate, AncillaryServiceOffer aggregate, compliance tracking, demand response administration, balancing authority operations

**Does not own**: Physical generation dispatch (Generation Context), market settlement (Trading & Market Context), customer metering (Metering & Billing Context), grid operations (Transmission & Distribution Context)

**Upstream contexts**: External ISO/RTO (market rules, reliability standards), FERC/NERC (regulatory requirements), state PUCs (tariff approvals, RPS mandates)

**Downstream contexts**: Generation Context (dispatch orders, capacity requirements), Trading & Market Context (market rules, position limits), Metering & Billing Context (tariff schedules), Transmission & Distribution Context (reliability standards)

## Core Capabilities

### Compliance Management

Compliance management tracks regulatory obligations and ensures timely, accurate reporting.

- Maintain a registry of all applicable NERC reliability standards with their requirements and evidence expectations
- Track compliance status for each standard: compliant, gap identified, remediation in progress, violation reported
- Coordinate evidence collection across Generation, T&D, and Trading contexts
- Prepare and submit compliance filings to NERC regional entities
- Manage FERC reporting requirements including Form 714 (annual demand and energy), Form 1 (financial and operational), and EQR (Electric Quarterly Report)
- Track state-level compliance obligations for environmental, consumer protection, and renewable portfolio standards
- Manage audit preparation and response for NERC CIP and operations and planning (O&P) audits

### Demand Response Administration

Demand response administration manages programs that incentivize consumers to reduce or shift electricity usage.

- Define demand response programs with enrollment criteria, notification requirements, and compensation structures
- Enroll eligible customers and resources into demand response programs
- Issue demand response event notifications when grid conditions or prices trigger activation
- Measure and verify participant load reduction using baseline methodologies
- Calculate demand response settlements and credits for participating customers
- Report program performance to regulators and ISO/RTO market operators

### Ancillary Services

Ancillary services ensure grid reliability through frequency regulation, reserves, and voltage support.

- Determine ancillary service requirements based on grid conditions and NERC/ISO mandates
- Qualify generating units and demand-side resources for ancillary service provision
- Submit ancillary service offers to ISO/RTO markets
- Monitor real-time performance of resources providing frequency regulation and spinning reserves
- Track ancillary service deployment events and calculate performance payments

### Capacity Planning and Markets

Capacity planning ensures long-term adequacy of generation resources to meet future demand.

- Participate in capacity market auctions (e.g., PJM RPM, ISO-NE FCM) by offering generation and demand-side resources
- Track capacity commitments with delivery periods, obligation quantities, and performance requirements
- Monitor capacity obligation compliance and assess penalty exposure for non-performance
- Coordinate with the Generation Context to ensure committed resources are available during delivery periods
- Report capacity position to regulators and ISO/RTO capacity market administrators

### Balancing Authority Operations

Balancing authority operations maintain the instantaneous balance between supply and demand.

- Monitor area control error (ACE) and maintain compliance with NERC BAL standards
- Coordinate generation dispatch to maintain frequency within acceptable limits
- Manage interchange schedules with neighboring balancing authorities
- Activate operating reserves when generation loss or demand spikes exceed normal regulation range
- Report balancing performance metrics to NERC and the regional reliability coordinator

## Key Aggregates and Entities

- **ComplianceObligation** (aggregate root): A specific regulatory requirement to be met, identified by ObligationId
- **DemandResponseProgram** (aggregate root): A demand response program definition with enrollment and event rules
- **CapacityCommitment** (aggregate root): A capacity market obligation for a specific delivery period
- **AncillaryServiceOffer** (aggregate root): An offer to provide ancillary services to the ISO/RTO
- **DemandResponseEvent** (entity within DemandResponseProgram): A specific activation of demand response
- **ComplianceEvidence** (value object): Documentation demonstrating compliance with a standard
- **AreaControlError** (value object): The current imbalance between scheduled and actual interchange

## Domain Events Produced

- **DemandResponseActivated**: A demand response event has been triggered
- **ComplianceViolationDetected**: A regulatory standard has been breached
- **AncillaryServiceDeployed**: An ancillary service has been called upon
- **CapacityCommitmentRecorded**: A capacity obligation has been recorded
- **TariffApproved**: A new or modified tariff has been approved by regulators
- **ReliabilityDispatchOrdered**: An emergency dispatch order has been issued for grid reliability
- **MarketRuleChanged**: An ISO/RTO market rule has been updated

## Domain Events Consumed

- **UnitDispatched** (from Generation Context): Generation dispatch data for compliance verification
- **OutageDetected** (from Transmission & Distribution Context): Grid events for reliability reporting
- **TradeExecuted** (from Trading & Market Context): Market activity for position limit monitoring
- **MeterReadingReceived** (from Metering & Billing Context): Consumption data for demand response verification

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Regulatory & Grid is one of six core bounded contexts
- [Generation Context](../generation-context/) -- Receives reliability dispatch orders and capacity requirements
- [Trading & Market Context](../trading-market-context/) -- Receives market rules and position limits
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACL translates ISO/RTO interfaces
- [Regulatory Compliance](../regulatory-compliance/) -- Detailed regulatory requirements
- [Energy Standards](../energy-standards/) -- NERC CIP, NAESB standards

## References

- NERC Reliability Standards. "NERC Standards." North American Electric Reliability Corporation. https://www.nerc.com/pa/Stand/Pages/AllReliabilityStandards.aspx
- FERC. "Federal Energy Regulatory Commission Regulations." 18 CFR Parts 35, 40, 284.
- Kirschen, Daniel S. & Strbac, Goran. "Fundamentals of Power System Economics." 2nd Edition. Wiley, 2018.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
