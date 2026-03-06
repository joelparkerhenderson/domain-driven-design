# Repositories in Energy

## Overview

A repository is an abstraction that encapsulates the logic for storing, retrieving, and searching for aggregate roots. Repositories provide a collection-like interface to the domain layer, hiding the details of the underlying persistence mechanism. The domain model interacts with repositories as if they were in-memory collections, keeping business logic pure and independent of database technology. In the energy domain, repositories manage the persistence of generating units, grid assets, contracts, meters, compliance records, and renewable assets.

## Relevance to Energy

Energy systems manage vast amounts of persistent data: decades of generation history, millions of meter readings, thousands of transmission line records, and complex contract portfolios. Repositories ensure that the domain model does not become entangled with database queries, ORM configurations, or data access patterns. This separation allows the domain logic (dispatch optimization, settlement calculation, compliance checking) to remain pure and testable.

## Energy Repositories

### Generation Context Repositories

- **GeneratingUnitRepository**: Retrieves and persists GeneratingUnit aggregates. Supports queries by unit ID, plant ID, fuel type, operating status, and capacity range. Enables finding all available units for dispatch optimization.
- **PowerPlantRepository**: Retrieves PowerPlant aggregates with their associated unit references. Supports queries by location, fuel type, and total capacity.
- **FuelContractRepository**: Manages FuelContract aggregates. Supports queries by supplier, fuel type, contract period, and delivery status.

### Transmission & Distribution Context Repositories

- **TransmissionLineRepository**: Persists TransmissionLine aggregates. Supports queries by voltage level, endpoints, thermal limit, and operating status.
- **SubstationRepository**: Manages Substation aggregates. Supports queries by voltage level, geographic area, and equipment configuration.
- **OutageRepository**: Persists Outage aggregates. Supports queries by status (active, restored), affected area, duration, and cause. Enables retrieval of historical outage patterns.
- **DistributionFeederRepository**: Manages DistributionFeeder aggregates. Supports queries by substation, service area, and customer count.

### Trading & Market Context Repositories

- **EnergyContractRepository**: Persists EnergyContract aggregates. Supports queries by counterparty, contract type, delivery point, contract period, and status.
- **MarketPositionRepository**: Manages MarketPosition aggregates. Supports queries by delivery point, time period, and exposure level.
- **TradeExecutionRepository**: Persists TradeExecution records. Supports queries by trade date, counterparty, volume, and price range.
- **SettlementRepository**: Manages Settlement aggregates. Supports queries by settlement period, market participant, and net amount.

### Metering & Billing Context Repositories

- **MeterRepository**: Persists Meter aggregates. Supports queries by meter type (smart, conventional), communication status, service point, and installation date.
- **CustomerAccountRepository**: Manages CustomerAccount aggregates. Supports queries by account number, service address, tariff class, and account status.
- **InvoiceRepository**: Persists Invoice aggregates. Supports queries by account, billing period, payment status, and amount range.

### Regulatory & Grid Context Repositories

- **ComplianceObligationRepository**: Persists ComplianceObligation aggregates. Supports queries by standard (NERC, FERC), obligation status, due date, and severity.
- **DemandResponseProgramRepository**: Manages DemandResponseProgram aggregates. Supports queries by program type, enrollment status, and season.
- **CapacityCommitmentRepository**: Persists capacity market commitments. Supports queries by delivery period, commitment status, and capacity amount.

### Renewable Integration Context Repositories

- **RenewableAssetRepository**: Persists RenewableAsset aggregates. Supports queries by asset type (solar, wind), location, capacity, and operational status.
- **BatteryStorageSystemRepository**: Manages BatteryStorageSystem aggregates. Supports queries by capacity, state of charge range, and operational status.
- **GenerationForecastRepository**: Persists GenerationForecast aggregates. Supports queries by asset, forecast horizon, and confidence level.

## Repository Design Principles

- **One repository per aggregate root**: There is no repository for entities or value objects that exist within an aggregate. MeterReading is accessed through the MeterRepository, not through its own repository.
- **Collection-like interface**: Repositories provide add, remove, and find operations. The domain layer does not know about SQL, NoSQL, or any specific persistence technology.
- **Query specification**: Complex queries (e.g., "find all generating units with capacity above 100 MW that are available for dispatch") are expressed as domain-meaningful specifications, not as raw database queries.
- **Unit of work**: Repository save operations coordinate with the unit of work pattern to ensure that aggregate changes and domain events are persisted atomically.
- **No business logic**: Repositories contain no business rules. They store and retrieve aggregates; the aggregates contain the business logic.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Repositories persist and retrieve aggregate roots
- [Entities](../entities/) -- Repositories find entities by their unique identity
- [Domain Events](../domain-events/) -- Repositories may store domain events alongside aggregates (event sourcing)
- [Domain Services](../domain-services/) -- Domain services use repositories to load aggregates for cross-aggregate operations
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context has its own set of repositories

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 6: The Life Cycle of a Domain Object. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12: Repositories. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Aggregates. O'Reilly, 2021.
- Fowler, Martin. "Patterns of Enterprise Application Architecture." Chapter 10: Data Source Architectural Patterns. Addison-Wesley, 2002.
