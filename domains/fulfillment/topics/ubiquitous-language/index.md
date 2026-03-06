# Ubiquitous Language in Fulfillment

## Overview

Ubiquitous language is the shared vocabulary between developers, domain experts, and stakeholders within a bounded context. Every term has one precise meaning, used consistently in code, conversation, documentation, and user interfaces. In fulfillment, where multiple operational disciplines converge, maintaining a precise ubiquitous language within each bounded context prevents the confusion that arises when terms like "order," "shipment," and "status" are overloaded with different meanings.

## Relevance to Fulfillment

Fulfillment operations bring together order management teams, warehouse personnel, shipping clerks, logistics coordinators, delivery drivers, and customer service agents. Each group has its own jargon. Without a ubiquitous language:

- A "pick" might mean selecting an item from a bin (warehouse) or choosing a carrier rate (shipping)
- An "allocation" might mean reserving inventory (inventory team) or assigning a picker to a wave (warehouse team)
- A "return" might mean a customer-initiated return (customer service) or a failed delivery returned to depot (delivery team)

The ubiquitous language resolves these ambiguities by defining terms precisely within each bounded context.

## Language by Bounded Context

### Order Processing Context

- **FulfillmentOrder**: A validated customer order that has been accepted for fulfillment, distinct from the sales order that originated it
- **OrderLine**: A single item and quantity within a fulfillment order
- **OrderHold**: A temporary suspension of order processing due to fraud review, address validation failure, or payment issues
- **OrderSplit**: The division of a single customer order into multiple fulfillment orders, typically because items are in different warehouses
- **RoutingRule**: Business logic that determines which warehouse or fulfillment center should process a given order
- **OrderCancellation**: The act of stopping fulfillment of an order before it has been picked
- **OrderPriority**: Classification of order urgency (standard, expedited, same-day) that affects processing sequence
- **ChannelSource**: The originating sales channel (e-commerce site, marketplace, ERP, retail store)

### Inventory Allocation Context

- **InventoryAllocation**: The reservation of specific inventory quantities against a fulfillment order
- **SoftAllocation**: A tentative reservation that holds stock for a limited time pending order confirmation
- **HardAllocation**: A committed reservation that removes stock from available-to-sell quantities
- **AvailableToSell (ATS)**: The quantity of a SKU that can be promised to new orders, accounting for on-hand stock minus existing allocations and safety stock
- **StockKeepingUnit (SKU)**: The unique identifier for a distinct product variant
- **Backorder**: An order accepted when stock is insufficient, to be fulfilled when inventory is replenished
- **SafetyStock**: A minimum inventory buffer maintained to protect against demand variability and supply chain disruptions
- **ReorderPoint**: The inventory level at which a replenishment order should be triggered
- **CycleCount**: A periodic audit of a subset of inventory to maintain accuracy without a full physical count
- **InventoryReconciliation**: The process of resolving discrepancies between system inventory records and physical counts

### Warehouse Operations Context

- **PickWave**: A batch of pick tasks grouped for efficient execution, typically optimized by zone, priority, or carrier cutoff time
- **PickList**: The detailed list of items, quantities, and bin locations for a picker to collect
- **PickPath**: The optimized route through the warehouse that minimizes travel distance for a picker
- **BinLocation**: A specific storage position within the warehouse, identified by aisle, rack, shelf, and position
- **Zone**: A logical grouping of bin locations, often organized by product type, temperature requirement, or pick frequency
- **PutAway**: The process of moving received goods from the receiving dock to their designated storage locations
- **PackStation**: A workstation where picked items are verified, packed into shipping containers, and prepared for labeling
- **StagingArea**: A designated area where packed orders await carrier pickup or loading
- **ShortPick**: A situation where a picker cannot find the expected quantity at the bin location
- **Receiving**: The process of accepting inbound inventory shipments, verifying quantities, and recording receipt
- **WaveRelease**: The act of releasing a pick wave to the warehouse floor for execution

### Shipping & Carrier Context

- **Shipment**: A collection of one or more packages assigned to a single carrier service for delivery to one destination
- **Package**: A physical container (box, envelope, pallet) with defined weight and dimensions, containing one or more items
- **ShippingLabel**: A carrier-generated label containing destination address, tracking barcode, routing information, and service level
- **Manifest**: A document listing all packages to be picked up by a carrier in a single collection event
- **CarrierRate**: The cost quoted by a carrier for transporting a package, based on weight, dimensions, origin, destination, and service level
- **ServiceLevel**: The speed and handling classification for a shipment (ground, two-day, overnight, freight, white glove)
- **RateShop**: The process of comparing rates across multiple carriers and service levels to find the optimal shipping option
- **Dispatch**: The act of releasing a shipment to the carrier for transportation
- **CustomsDeclaration**: Documentation required for international shipments describing contents, value, and country of origin

### Delivery & Tracking Context

- **TrackingNumber**: A unique identifier assigned by the carrier to a shipment, used to monitor delivery progress
- **TrackingEvent**: A timestamped status update from the carrier indicating a change in shipment location or status
- **DeliveryAttempt**: A single effort by the carrier to deliver a package to the recipient
- **ProofOfDelivery (POD)**: Evidence that a package was successfully delivered, such as a signature, photograph, or GPS confirmation
- **DeliveryException**: An event indicating a problem with delivery, such as address not found, recipient unavailable, or damaged package
- **EstimatedDeliveryDate (EDD)**: The predicted date of delivery, calculated from carrier transit times and service level
- **LastMile**: The final leg of delivery from the local distribution center to the customer's address
- **InTransit**: The status of a shipment between dispatch from the fulfillment center and arrival at the delivery destination

### Returns & Exceptions Context

- **ReturnRequest**: A customer-initiated request to return one or more items from a delivered order
- **ReturnMerchandiseAuthorization (RMA)**: A formal approval granting the customer permission to return items, including a return label and instructions
- **ReturnLabel**: A prepaid shipping label provided to the customer for returning items
- **Inspection**: The examination of returned items to determine condition, verify reason for return, and decide disposition
- **Disposition**: The decision on what to do with a returned item: restock, refurbish, liquidate, or dispose
- **Refund**: A monetary credit issued to the customer for returned items
- **Exchange**: A replacement order created when a customer returns an item and receives a different item in its place
- **ExceptionCase**: A non-standard situation requiring manual intervention, such as a lost shipment, damaged goods, or wrong item shipped

## Language Governance

### Glossary Maintenance

Each bounded context team maintains its own glossary. Terms are added when new concepts emerge from knowledge crunching sessions and are reviewed during sprint retrospectives.

### Cross-Context Translation

When concepts must cross context boundaries, explicit translation is defined. For example, when a FulfillmentOrder (Order Processing Context) becomes a PickList (Warehouse Operations Context), the mapping between order lines and pick tasks is documented and maintained in the integration layer.

### Code-Language Alignment

Class names, method names, variable names, and database column names use the ubiquitous language terms exactly. If the business says "PickWave," the code has a `PickWave` class, not a `BatchJob` or `WorkOrder` class.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Each bounded context defines its own ubiquitous language
- [Strategic Design](../strategic-design/) -- Ubiquitous language emerges from knowledge crunching during strategic design
- [Entities](../entities/) -- Entity names come from the ubiquitous language
- [Value Objects](../value-objects/) -- Value object names come from the ubiquitous language
- [Aggregates](../aggregates/) -- Aggregate names and invariant descriptions use the ubiquitous language
- [Domain Events](../domain-events/) -- Event names use the ubiquitous language in past tense

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 2: Communication and the Use of Language. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 1: Getting Started with DDD. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 2: Discovering Domain Knowledge. O'Reilly, 2021.
- Evans, Eric. "Domain-Driven Design Reference." Domain Language, Inc., 2015.
