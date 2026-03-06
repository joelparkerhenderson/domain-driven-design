# Domain Services in Enterprise Resource Planning

## Overview

A domain service is a stateless operation that belongs to the domain but does not naturally fit within a single entity or value object. In ERP, domain services handle cross-aggregate operations like pricing calculations, credit checks, and inventory allocation.

## ERP Domain Services

### PricingService

- **Purpose**: Calculates the final price for a sales order line item
- **Inputs**: Product, customer, quantity, order date
- **Logic**: Base price lookup → volume discount → customer-specific discount → promotional pricing → tax calculation
- **Output**: LineItemPricing value object with unit price, discount, tax, and total
- **Aggregates involved**: PriceList, Customer, SalesOrder

### CreditCheckService

- **Purpose**: Verifies a customer's order does not exceed their credit limit
- **Inputs**: Customer, proposed order total
- **Logic**: Current AR balance + open order totals + proposed order ≤ credit limit
- **Output**: CreditCheckResult (approved, rejected with reason, requires override)
- **Aggregates involved**: Customer, SalesOrder (multiple)

### InventoryAllocationService

- **Purpose**: Reserves inventory for a sales order across warehouses
- **Inputs**: Order line items, preferred warehouse, allocation strategy
- **Logic**: Check preferred warehouse → check alternate warehouses → partial allocation → backorder remainder
- **Output**: AllocationResult with reserved quantities per warehouse
- **Aggregates involved**: InventoryItem (multiple), SalesOrder

### MRPService (Material Requirements Planning)

- **Purpose**: Calculates material needs based on production plans and current inventory
- **Inputs**: Production plan, BOMs, current inventory levels, open purchase orders
- **Logic**: Explode BOMs → net against inventory → net against open POs → generate purchase requisitions for shortages
- **Output**: List of PurchaseRequisition commands
- **Aggregates involved**: BillOfMaterials, InventoryItem, PurchaseOrder, WorkOrder

### PayrollCalculationService

- **Purpose**: Calculates gross-to-net pay for an employee
- **Inputs**: Employee, pay period, hours worked, overtime, deductions
- **Logic**: Gross pay → federal tax → state tax → FICA → benefits deductions → garnishments → net pay
- **Output**: PayStub value object
- **Aggregates involved**: Employee, PayrollRun

### FiscalPeriodCloseService

- **Purpose**: Performs period-end closing procedures
- **Inputs**: Fiscal period, general ledger accounts
- **Logic**: Verify all entries posted → calculate depreciation → accrue expenses → close temporary accounts → generate trial balance
- **Output**: PeriodCloseResult with financial statements
- **Aggregates involved**: Account (multiple), JournalEntry

## When to Use Domain Services

- Operation spans multiple aggregates
- Operation represents a domain concept without a natural entity home
- Operation requires domain logic that would be awkward on a single entity

## Relationships to Other Topics

- [Aggregates](../aggregates/) — Domain services coordinate across aggregates
- [Repositories](../repositories/) — Services use repositories to load aggregates
- [Domain Events](../domain-events/) — Services may produce events as side effects
- [Value Objects](../value-objects/) — Services often return value objects

## References

- Evans, Eric. "Domain-Driven Design." Chapter 5. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 7. Addison-Wesley, 2013.
