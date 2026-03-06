# Trading & Market Context in Energy

## Overview

The Trading & Market Context is the bounded context responsible for wholesale energy market participation, energy contract management, financial settlement, and price risk management. It translates generation capacity and forecasted demand into market bids, executes trades on day-ahead and real-time markets, manages the resulting financial positions, and reconciles scheduled versus actual energy delivery through settlement processes.

## Relevance to Energy

Wholesale energy markets are the financial engine of the electricity industry. Billions of dollars in energy transactions clear daily through ISO/RTO-administered markets. The Trading & Market Context manages this financial complexity while maintaining clear separation from the physical operations of generation and grid management. It ensures that market participation decisions are informed by generation costs and grid constraints, but that market-specific logic (bidding strategies, risk limits, settlement rules) does not leak into operational contexts.

## Bounded Context Boundaries

**Owns**: EnergyContract aggregate, MarketPosition aggregate, TradeExecution aggregate, Settlement aggregate, bidding strategies, risk calculations, settlement reconciliation

**Does not own**: Physical generation dispatch (Generation Context), grid power flow (Transmission & Distribution Context), customer retail billing (Metering & Billing Context), market rule compliance (Regulatory & Grid Context)

**Upstream contexts**: Generation Context (available capacity, unit costs), Renewable Integration Context (forecasted renewable output), Regulatory & Grid Context (market rules, position limits)

**Downstream contexts**: Generation Context (dispatch instructions from market clearing), Metering & Billing Context (wholesale price data for large customers)

## Core Capabilities

### Market Bidding

Market bidding formulates and submits energy supply and demand bids to ISO/RTO-administered markets.

- Formulate day-ahead market bids based on generation portfolio costs, demand forecasts, and price expectations
- Submit real-time market offers reflecting current unit availability and marginal costs
- Calculate optimal bid curves considering unit heat rates, fuel costs, start-up costs, and no-load costs
- Manage bid submission deadlines and market participation requirements
- Handle multi-part bids (energy, start-up, no-load) and complex bid structures
- Track bid acceptance and clearing results

### Contract Management

Contract management handles the lifecycle of bilateral energy purchase and sale agreements.

- Manage power purchase agreements (PPAs) from negotiation through execution to expiration
- Track contract terms including volume, price, delivery point, and delivery schedule
- Handle contract amendments, extensions, and terminations
- Manage counterparty credit exposure and credit limit enforcement
- Support multiple contract types: fixed-price, index-based, tolling, heat-rate-based, and virtual PPAs

### Settlement

Settlement reconciles the financial obligations between market participants based on actual delivery.

- Calculate day-ahead market settlement based on cleared bids and actual delivery
- Calculate real-time market settlement for imbalance quantities at real-time prices
- Apply uplift charges, congestion credits, and loss credits
- Reconcile scheduled vs. actual generation and consumption
- Generate settlement statements for each market participant
- Handle settlement disputes and adjustments

### Risk Management

Risk management monitors and controls the financial exposure of the trading portfolio.

- Calculate mark-to-market value of the open position portfolio
- Monitor position limits and credit exposure against approved thresholds
- Perform value-at-risk (VaR) analysis and stress testing
- Track hedge effectiveness for physical and financial positions
- Generate risk reports for management and regulatory compliance

## Key Aggregates and Entities

- **EnergyContract** (aggregate root): A bilateral agreement to buy or sell energy, identified by ContractId
- **MarketPosition** (aggregate root): The net financial exposure at a delivery point and period
- **TradeExecution** (aggregate root): A specific market trade recording the execution details
- **Settlement** (aggregate root): The financial reconciliation for a settlement period and participant
- **ContractPrice** (value object): The agreed price including type (fixed, index, formula) and currency
- **LMP** (value object): The locational marginal price at a grid node
- **SettlementAmount** (value object): A calculated financial amount from settlement reconciliation
- **DeliveryPoint** (value object): The grid location where energy is delivered or received

## Domain Events Produced

- **TradeExecuted**: A market trade has been completed
- **MarketPricePublished**: A new LMP has been published for a grid node
- **SettlementCalculated**: Financial settlement for a period has been computed
- **ContractExecuted**: A new bilateral contract has been finalized
- **PositionLimitBreached**: Trading exposure has exceeded approved limits
- **DispatchInstructionIssued**: Market clearing results direct generation dispatch

## Domain Events Consumed

- **UnitDispatched** (from Generation Context): Actual generation dispatch affecting settlement
- **CapacityChanged** (from Generation Context): Available capacity changes affecting bidding
- **GenerationForecastUpdated** (from Renewable Integration Context): Renewable forecasts affecting market position
- **MarketRuleChanged** (from Regulatory & Grid Context): ISO/RTO rule changes affecting bidding and settlement
- **DemandForecastUpdated** (from Regulatory & Grid Context): Load forecasts affecting price expectations

## Anti-Corruption Layer

The Trading & Market Context maintains an ACL for each ISO/RTO market platform it interacts with.

- Translates ISO-specific bid submission formats into internal MarketBid representations
- Maps ISO settlement line items to internal Settlement value objects
- Converts ISO node identifiers to internal grid node references
- Handles protocol differences across ISOs (PJM, ERCOT, CAISO, MISO, ISO-NE, NYISO, SPP)

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Trading & Market is one of six core bounded contexts
- [Generation Context](../generation-context/) -- Upstream source of capacity and cost data
- [Regulatory & Grid Context](../regulatory-grid-context/) -- Source of market rules and compliance requirements
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACL translates ISO/RTO market interfaces
- [Domain Events](../domain-events/) -- Produces trade, price, and settlement events
- [Aggregates](../aggregates/) -- Owns EnergyContract, MarketPosition, and Settlement aggregates

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Kirschen, Daniel S. & Strbac, Goran. "Fundamentals of Power System Economics." 2nd Edition. Wiley, 2018.
- Harris, Chris. "Electricity Markets: Pricing, Structures and Economics." Wiley Finance, 2006.
- Stoft, Steven. "Power System Economics: Designing Markets for Electricity." IEEE Press, 2002.
