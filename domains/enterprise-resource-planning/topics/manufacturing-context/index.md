# Manufacturing Context in Enterprise Resource Planning

## Overview

The Manufacturing Context manages the production of goods: production planning, bills of materials, work orders, routing, shop floor execution, quality control, and capacity planning. It transforms raw materials and components into finished products.

## Core Responsibilities

- **Production Planning**: Scheduling production runs based on demand forecasts, sales orders, and inventory levels
- **Bill of Materials (BOM)**: Defining the hierarchical structure of components, subassemblies, and raw materials needed to manufacture a product
- **Work Order Management**: Creating, scheduling, and tracking production work orders
- **Routing**: Defining the sequence of operations, work centers, and processing times for manufacturing
- **Shop Floor Execution**: Recording material consumption, labor time, and production output
- **Quality Control**: Inspecting materials and finished goods, managing holds, tracking defects
- **Capacity Planning**: Balancing production demand against available work center capacity

## Aggregates

### WorkOrder Aggregate

- **Root**: WorkOrder (identified by WorkOrderID)
- **Internal entities**: OperationStep, MaterialConsumption
- **Value objects**: Quantity (planned, completed, scrapped), RoutingReference, BOMReference, ScheduledDates
- **States**: Planned → Released → In Progress → Completed → Closed
- **Invariants**: Material consumption cannot exceed allocated plus tolerance; completed quantity cannot exceed planned without override; quality hold prevents release

### BillOfMaterials Aggregate

- **Root**: BillOfMaterials (identified by Product + Version)
- **Internal entities**: BOMLine (component, quantity per unit, unit of measure, scrap factor)
- **Value objects**: Quantity, EffectiveDateRange
- **Invariants**: BOM cannot reference itself (no circular dependencies); all components must be valid products; quantity per unit must be positive

### ProductionPlan Aggregate

- **Root**: ProductionPlan (identified by PlanID, planning period)
- **Internal entities**: PlannedOrder
- **Value objects**: Quantity, DateRange, DemandSource
- **Invariants**: Planned quantities must be achievable within capacity constraints; material availability verified

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| WorkOrderCreated | Production scheduled | Inventory (material reservation) |
| ProductionStarted | Manufacturing begins | Inventory (material issuance), Costing |
| MaterialConsumed | Raw materials used | Inventory (stock reduction) |
| ProductionCompleted | Finished goods produced | Inventory (receipt), Finance (cost roll-up) |
| QualityHoldApplied | Quality issue detected | Production pause, Investigation |
| QualityReleased | Quality approved | Inventory (available for sale) |
| ScrapRecorded | Defective units identified | Finance (scrap cost), Quality reporting |

## Integration with Other Contexts

- **Manufacturing → Inventory**: Consumes raw materials (issuance), produces finished goods (receipt)
- **Manufacturing → Procurement**: MRP generates purchase requisitions for material shortages
- **Manufacturing → Finance**: Production costs (materials, labor, overhead) create WIP and COGS entries
- **Sales → Manufacturing**: Customer orders drive production planning (make-to-order)
- **Inventory → Manufacturing**: Material availability constrains production scheduling

## Key Concepts

### MRP (Material Requirements Planning)

Calculates what materials are needed, in what quantities, and when, by exploding BOMs against production plans, netting against inventory, and generating procurement and production orders for shortages.

### Production Costing

- **Material cost**: Actual cost of raw materials consumed
- **Labor cost**: Direct labor hours × labor rate
- **Overhead**: Allocated manufacturing overhead based on machine hours or labor hours
- **Standard cost vs. actual cost**: Variance analysis drives continuous improvement

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six ERP bounded contexts
- [Context Map](../context-map/) — Partnership with Inventory; upstream to Procurement
- [Aggregates](../aggregates/) — WorkOrder, BillOfMaterials, ProductionPlan are aggregate roots
- [Domain Services](../domain-services/) — MRPService coordinates material planning
- [Domain Events](../domain-events/) — Manufacturing events drive Inventory and Finance

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vollmann, Berry, Whybark, & Jacobs. "Manufacturing Planning and Control for Supply Chain Management." McGraw-Hill, 2010.
- Monk, Ellen & Wagner, Bret. "Concepts in Enterprise Resource Planning." Cengage Learning, 2012.
