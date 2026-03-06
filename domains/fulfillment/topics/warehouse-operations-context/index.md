# Warehouse Operations Context in Fulfillment

## Overview

The Warehouse Operations Context is the bounded context that models the physical processes occurring inside a fulfillment center: generating pick waves, executing pick/pack/stage workflows, managing bin locations and storage zones, receiving inbound inventory, and directing put-away. It translates allocation decisions into physical warehouse activities performed by warehouse personnel, and it reports completion events that advance orders through the fulfillment pipeline.

## Relevance to Fulfillment

Warehouse operations are where digital order data meets physical logistics. The efficiency of picking, packing, and staging directly determines fulfillment throughput, labor cost, and delivery speed. This bounded context captures the domain knowledge of warehouse managers, pickers, packers, and receiving clerks, modeling their workflows in the ubiquitous language they use daily.

## Bounded Context Boundaries

**Owns**: PickWave aggregate, PickList entity, bin location model, receiving and put-away workflows, pack station operations, staging area management

**Does not own**: Inventory availability calculations (Inventory Allocation Context), carrier selection and label generation (Shipping & Carrier Context), order intake and validation (Order Processing Context)

**Upstream contexts**: Inventory Allocation Context (provides allocation decisions that trigger pick tasks)

**Downstream contexts**: Shipping & Carrier Context (receives packed shipments for labeling and dispatch), Inventory Allocation Context (receives receiving events and short-pick reports)

## Core Capabilities

### Pick Wave Management

Pick waves group multiple orders into batches that can be picked efficiently by warehouse staff during a single pass through the warehouse.

- **Wave planning**: Group orders by zone, carrier cutoff time, service level priority, and shipping destination to create efficient pick waves
- **Wave release**: Release planned waves to the warehouse floor based on labor availability and carrier pickup schedules
- **Wave monitoring**: Track wave progress in real time, including pick completion rates and short-pick counts
- **Wave completion**: Close a wave when all pick lists within it are completed or resolved

### Pick Operations

Picking is the process of retrieving items from their storage locations based on pick lists.

- **Pick list generation**: Create pick lists from allocation data, assigning specific bin locations and quantities to each list
- **Pick path optimization**: Sequence bin locations on each pick list to minimize travel distance through the warehouse
- **Zone picking**: Assign pick lists to specific warehouse zones so pickers operate in a focused area
- **Batch picking**: Combine items from multiple orders onto a single pick list when they share bin locations
- **Short pick handling**: When a bin contains fewer items than expected, record the short pick, notify the Inventory Allocation Context, and trigger cycle count or reallocation

### Pack Operations

Packing is the process of placing picked items into shipping containers at pack stations.

- **Pack station assignment**: Route picked items to available pack stations based on order priority and station capability
- **Container selection**: Select the appropriate box, envelope, or packaging type based on item dimensions, weight, and fragility
- **Pack verification**: Scan each item during packing to verify correctness against the order
- **Packing slip insertion**: Generate and include packing slips, invoices, or promotional inserts
- **Weight capture**: Weigh each packed container and record actual weight for carrier billing accuracy

### Staging

Staging is the process of organizing packed containers in the shipping dock area for carrier pickup.

- **Carrier-based staging**: Group packed containers by carrier and service level in designated staging lanes
- **Cutoff management**: Prioritize staging for shipments approaching carrier cutoff times
- **Dock door assignment**: Direct staged shipments to the appropriate dock door for carrier pickup

### Receiving and Put-Away

Receiving handles inbound inventory arriving from suppliers, transfers, or customer returns.

- **Receiving verification**: Count and inspect incoming items against purchase orders or advance ship notices (ASN)
- **Quality inspection**: Flag items that arrive damaged or do not match specifications
- **Put-away direction**: Assign received items to specific bin locations based on velocity, product category, and available space
- **Bin optimization**: Place high-velocity SKUs in easily accessible pick locations to improve pick efficiency

### Bin Management

Bin management maintains the warehouse storage model, tracking what is stored where.

- **Bin location model**: Hierarchical model of warehouse zones, aisles, racks, shelves, and bins
- **Bin capacity tracking**: Monitor available space in each bin to prevent over-stocking
- **Bin replenishment**: Trigger replenishment from reserve storage to forward pick locations when bins fall below threshold
- **Cycle counting**: Schedule and execute targeted inventory counts at the bin level to maintain accuracy

## Key Aggregates and Entities

- **PickWave** (aggregate root): A batch of pick lists grouped for efficient warehouse execution
- **PickList** (entity within PickWave): A set of items to be picked from specific bin locations by a single picker
- **BinLocation** (value object): A specific storage position within the warehouse hierarchy
- **ZoneAssignment** (value object): Maps a pick list to a warehouse zone
- **PickerAssignment** (value object): Associates a picker with a pick list

## Domain Events Produced

- **WavePlanned**: A new pick wave has been created and is ready for release
- **WaveReleased**: A pick wave has been released to the warehouse floor
- **PickStarted**: A picker has begun working on a pick list
- **PickCompleted**: A picker has finished all items on a pick list
- **ShortPickReported**: A bin contained fewer items than expected during picking
- **PackCompleted**: Items have been packed into a shipping container
- **ItemReceived**: Inbound inventory has been received and verified at the warehouse

## Domain Events Consumed

- **InventoryAllocated** (from Inventory Allocation Context): Triggers creation of pick tasks
- **AllocationReleased** (from Inventory Allocation Context): Cancels pick tasks for released allocations
- **ShipmentLabeled** (from Shipping & Carrier Context): Triggers staging of labeled packages

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Warehouse Operations is one of the six core bounded contexts
- [Inventory Allocation Context](../inventory-allocation-context/) -- Upstream context providing allocation decisions
- [Shipping & Carrier Context](../shipping-carrier-context/) -- Downstream context receiving packed shipments
- [Domain Events](../domain-events/) -- Produces PickCompleted, PackCompleted, ItemReceived events
- [Aggregates](../aggregates/) -- Owns the PickWave aggregate
- [Entities](../entities/) -- Defines the PickList entity and Warehouse reference entity

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 14: Maintaining Model Integrity. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 7: Modeling Bounded Contexts. O'Reilly, 2021.
- Bartholdi, John J. & Hackman, Steven T. "Warehouse & Distribution Science." Georgia Institute of Technology, 2019.
