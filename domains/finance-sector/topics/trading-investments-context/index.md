# Trading & Investments Context

## Overview

The Trading & Investments context manages the buying and selling of financial instruments. It handles order management, trade execution, portfolio management, market data consumption, position tracking, and trade settlement. This context operates in a high-throughput, low-latency environment where correctness and speed are both critical.

## Core Concepts

### Order Management

An order is an instruction to buy or sell a specific quantity of a financial instrument at a specified price or at the current market price.

- **Market order**: Execute immediately at the best available price
- **Limit order**: Execute at the specified price or better
- **Stop order**: Trigger execution when the market reaches a specified price
- **Order routing**: Directing orders to the appropriate exchange, dark pool, or market maker for optimal execution

### Trade Execution

A trade is the result of an order being matched and filled. Trade execution captures:

- The security traded (identified by ISIN, CUSIP, or ticker)
- Quantity and price at which the trade was filled
- Counterparty and execution venue
- Trade date and settlement date
- Commissions and fees

### Portfolio Management

Portfolio management involves constructing and maintaining collections of financial instruments to achieve investment objectives:

- **Position tracking**: Maintaining the current quantity and cost basis of each holding
- **Asset allocation**: Distributing investments across asset classes (equities, fixed income, alternatives)
- **Rebalancing**: Adjusting portfolio weights to maintain target allocations
- **Performance measurement**: Calculating returns, benchmarking, and attribution analysis

### Market Data

Real-time and historical pricing, volume, and reference data for financial instruments:

- **Level 1 data**: Best bid/offer prices and last trade price
- **Level 2 data**: Full order book depth showing all bids and offers
- **Reference data**: Security master information including instrument type, issuer, terms, and identifiers
- **Corporate actions**: Dividends, stock splits, mergers, and other events affecting securities

### Settlement

The final exchange of securities and cash between buyer and seller to complete a trade:

- **T+1 / T+2**: Standard settlement cycles (trade date plus one or two business days)
- **DVP (Delivery versus Payment)**: Simultaneous exchange of securities and cash
- **Fails management**: Handling trades that do not settle on the expected date
- **Custody**: Safekeeping of securities at a custodian bank or central securities depository

## Aggregates in This Context

### Order Aggregate

- **Root**: Order (OrderID)
- **Contains**: OrderLeg, FillRecord
- **Value objects**: Quantity, Price, SecurityIdentifier, OrderType, OrderStatus
- **Key invariant**: Filled quantity cannot exceed ordered quantity; cancelled orders cannot receive additional fills

### Trade Aggregate

- **Root**: Trade (TradeID)
- **Contains**: TradeLeg, Allocation
- **Value objects**: Money, Quantity, Price, SecurityIdentifier, SettlementDate
- **Key invariant**: All allocations must sum to total trade quantity; settlement date must be a valid business day

### Portfolio Aggregate

- **Root**: Portfolio (PortfolioID)
- **Contains**: Position, AllocationTarget
- **Value objects**: Money, Percentage, SecurityIdentifier
- **Key invariant**: Position quantities reflect all settled and pending trades; allocation percentages sum to 100%

### SecurityMaster Aggregate

- **Root**: Security (SecurityID)
- **Contains**: SecurityTerms, CorporateAction
- **Value objects**: ISIN, CUSIP, Ticker, SecurityType, MaturityDate
- **Key invariant**: Security must have at least one valid identifier; terms must be consistent with security type

## Domain Events

- OrderSubmitted -- new order entered into the order management system
- TradeExecuted -- order matched and filled at a specific price
- TradeAllocated -- block trade distributed across client portfolios
- TradeSettled -- securities and cash exchanged; consumed by Accounts & Ledger
- PositionUpdated -- portfolio position changed due to trade, corporate action, or valuation
- MarketDataReceived -- new price or reference data received from market data provider
- CorporateActionProcessed -- dividend, split, or other corporate action applied to positions

## Integration with Other Contexts

- **Accounts & Ledger** (downstream): TradeSettled events create journal entries for securities and cash movements
- **Risk & Compliance** (upstream): Pre-trade risk checks validate orders against position limits, margin requirements, and regulatory constraints
- **Payments & Transfers**: Cash settlement legs flow through the payment infrastructure
- **Customer Onboarding**: Client suitability profiles inform investment product eligibility

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- One of the six bounded contexts in the finance domain
- [Aggregates](../aggregates/) -- Order, Trade, Portfolio, and SecurityMaster aggregates defined here
- [Value Objects](../value-objects/) -- SecurityIdentifier, Price, Quantity are key value objects
- [Domain Events](../domain-events/) -- High-volume event production from trade execution and market data
- [Finance Standards](../finance-standards/) -- FIX protocol for electronic trading; ISO 20022 for settlement messages
- [Regulatory Compliance](../regulatory-compliance/) -- MiFID II (Europe), Reg NMS (US), and best execution requirements

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- FIX Trading Community. "FIX Protocol Specification." https://www.fixtrading.org/
- Hull, John C. "Options, Futures, and Other Derivatives." Pearson, 2021.
- DTCC. "Trade Lifecycle and Settlement." https://www.dtcc.com/
- CFA Institute. "Global Investment Performance Standards (GIPS)." https://www.gipsstandards.org/
