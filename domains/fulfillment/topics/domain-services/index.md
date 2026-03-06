# Domain Services in Fulfillment

## Overview

A domain service is a stateless operation that encapsulates domain logic which does not naturally belong to a single entity or value object. Domain services express significant business processes in the ubiquitous language, operating across multiple aggregates or requiring external information to make decisions. In fulfillment, domain services coordinate the cross-aggregate operations that drive the order-to-delivery pipeline -- allocating inventory, optimizing packing, selecting carriers, calculating routes, and authorizing returns.

## Relevance to Fulfillment

Fulfillment workflows frequently involve decisions that span multiple aggregates. Allocating inventory requires knowledge of available stock across warehouses, current allocations, and order priority. Selecting a carrier requires comparing rates, transit times, and service capabilities across multiple carriers for a given shipment. These operations do not belong to any single aggregate -- they are domain services that orchestrate information from several sources to produce a decision.

## Key Characteristics

- **Stateless**: Domain services hold no state between invocations. All required data is passed in or retrieved from repositories.
- **Named in the ubiquitous language**: Service names reflect business operations that domain experts recognize (e.g., "allocation," "rate shopping," "return authorization").
- **Defined in the domain layer**: Domain services are part of the domain model, not the application or infrastructure layer. They contain pure business logic.
- **Operate across aggregates**: When an operation involves multiple aggregates, a domain service coordinates the interaction.

## Fulfillment Domain Services

### AllocationService

**Purpose**: Determines how to reserve inventory for a fulfillment order by evaluating stock availability across warehouses, applying allocation rules, and producing allocation decisions.

**Operations**:

- `allocateOrder(fulfillmentOrder)`: Evaluates available stock across warehouses and allocates inventory to order lines based on proximity, stock levels, and priority rules
- `reallocate(fulfillmentOrder, reason)`: Re-runs allocation when initial allocation fails or when a warehouse becomes unavailable
- `releaseAllocation(allocationId)`: Releases previously allocated inventory back to available stock

**Business rules**:

- Prefer the warehouse closest to the shipping address to minimize transit time and cost
- Split orders across warehouses only when a single warehouse cannot fulfill the complete order
- Respect allocation priority: expedited orders are allocated before standard orders
- Soft allocations expire after a configurable timeout; hard allocations require explicit release

**Aggregates involved**: FulfillmentOrder, InventoryAllocation

### PackingService

**Purpose**: Determines optimal packing configurations for allocated items, selecting appropriate container types and grouping items to minimize shipping cost and material waste.

**Operations**:

- `determinePacking(pickedItems, availableContainers)`: Returns a packing plan specifying which items go into which containers
- `validatePacking(package)`: Verifies that a packed container meets carrier weight and dimension limits
- `estimatePackages(orderLines)`: Predicts the number and size of packages before picking, used for carrier rate estimation

**Business rules**:

- Minimize the number of packages to reduce shipping cost
- Do not exceed carrier weight or dimension limits for the selected service level
- Separate hazardous materials into dedicated packages per regulatory requirements
- Fragile items require appropriate packaging materials and may not be combined with heavy items

**Aggregates involved**: Shipment (Package creation), InventoryAllocation (item dimensions and weight)

### RouteCalculationService

**Purpose**: Determines the optimal fulfillment path for an order, including warehouse selection, shipping route, and estimated delivery timeline.

**Operations**:

- `calculateRoute(fulfillmentOrder)`: Returns the recommended warehouse, carrier, service level, and estimated delivery date
- `estimateDelivery(originWarehouse, destinationAddress, serviceLevel)`: Returns estimated transit days and delivery date
- `evaluateMultiWarehouseRoutes(fulfillmentOrder)`: Compares cost and speed of single-warehouse vs. split-warehouse fulfillment

**Business rules**:

- Balance delivery speed against shipping cost based on service level commitment
- Account for carrier cutoff times when selecting same-day dispatch options
- Factor in warehouse capacity and current workload when selecting fulfillment location
- International shipments must route through warehouses with customs clearance capabilities

**Aggregates involved**: FulfillmentOrder, Warehouse (reference data), Carrier (reference data)

### CarrierSelectionService

**Purpose**: Evaluates available carriers and service levels for a shipment, performing rate shopping to select the optimal carrier based on cost, transit time, and reliability.

**Operations**:

- `selectCarrier(shipment)`: Returns the recommended carrier and service level based on rate shopping results
- `getRates(shipment, carriers)`: Retrieves rate quotes from multiple carriers for comparison
- `validateCarrierEligibility(shipment, carrier)`: Confirms that a carrier can handle the shipment (size, weight, destination, hazmat)

**Business rules**:

- Select the lowest-cost carrier that meets the service level commitment
- Apply carrier preference rules (e.g., preferred carriers for specific regions or account discounts)
- Exclude carriers that do not service the destination address or cannot handle the package dimensions
- Fall back to alternate carriers when the primary carrier's API is unavailable
- Rate quotes expire after a configured duration and must be refreshed before label generation

**Aggregates involved**: Shipment, Carrier (reference data), CarrierRate (value object)

### ReturnAuthorizationService

**Purpose**: Evaluates return requests against return policies and order history, determining whether a return is authorized, what conditions apply, and how the return should be processed.

**Operations**:

- `authorizeReturn(returnRequest)`: Evaluates the return request against policies and returns an authorization decision
- `generateReturnLabel(returnRequest)`: Creates a return shipping label for authorized returns
- `determineDisposition(inspectionResult)`: Recommends disposition (restock, refurbish, dispose) based on inspection findings

**Business rules**:

- Returns must be requested within the return window (e.g., 30 days from delivery)
- Return quantity cannot exceed the originally shipped quantity for each line item
- Certain product categories (e.g., perishables, personalized items) may be non-returnable
- Free return shipping applies only to defective items or fulfillment errors
- Restocking fees may apply based on return reason and product condition

**Aggregates involved**: ReturnRequest, FulfillmentOrder (for order history validation)

## Domain Service Design Guidelines

1. **Do not put entity logic in services**: If an operation naturally belongs on an entity or aggregate, keep it there. Services are for cross-aggregate coordination.

2. **Keep services stateless**: All data needed for the operation should be passed as parameters or loaded from repositories within the service method.

3. **Return domain objects, not primitives**: An AllocationService returns InventoryAllocation aggregates, not raw database rows.

4. **Services produce domain events**: When a domain service completes a significant operation, it publishes a domain event (e.g., InventoryAllocated, CarrierSelected).

5. **Test services with domain objects**: Domain services should be testable with in-memory repositories and domain objects, without any infrastructure dependencies.

## Relationships to Other Topics

- [Aggregates](../aggregates/) -- Domain services coordinate operations across multiple aggregates
- [Repositories](../repositories/) -- Services use repositories to load and save aggregates
- [Domain Events](../domain-events/) -- Services produce events as outcomes of operations
- [Entities](../entities/) -- Services operate on entities but do not replace entity behavior
- [Value Objects](../value-objects/) -- Services use value objects as parameters and return values
- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own domain services

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7: Services. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Tactical Design Patterns. O'Reilly, 2021.
- Evans, Eric. "Domain-Driven Design Reference." Domain Language, Inc., 2015.
