# Repositories in Enterprise Resource Planning

## Overview

A repository encapsulates storage, retrieval, and search behavior for aggregate roots. In ERP, repositories provide domain-meaningful query interfaces for orders, customers, products, accounts, and other core aggregates.

## ERP Repositories

### SalesOrderRepository

- `findById(orderId)` — Retrieve specific order
- `findByCustomer(customerId, dateRange)` — Customer's orders over a period
- `findByStatus(status)` — Orders in a given state (open, shipped, cancelled)
- `findBackorders()` — Orders with unfulfilled line items
- `save(salesOrder)` — Persist new or updated order

### PurchaseOrderRepository

- `findByPoNumber(poNumber)` — Retrieve specific PO
- `findBySupplier(supplierId, dateRange)` — Supplier's POs over a period
- `findPendingDelivery()` — POs awaiting goods receipt
- `findAwaitingApproval()` — POs pending authorization
- `save(purchaseOrder)` — Persist PO

### CustomerRepository

- `findByCustomerNumber(number)` — Retrieve by business ID
- `findByName(name)` — Search by name
- `findWithOverdueInvoices()` — Customers with past-due balances
- `save(customer)` — Persist customer

### InventoryItemRepository

- `findBySku(sku, warehouseId)` — Retrieve stock record
- `findBelowReorderPoint(warehouseId)` — Items needing replenishment
- `findByProductCategory(category, warehouseId)` — Category-based queries
- `save(inventoryItem)` — Persist inventory record

### JournalEntryRepository

- `findByEntryId(entryId)` — Retrieve specific entry
- `findByAccount(accountNumber, fiscalPeriod)` — Entries for an account in a period
- `findUnposted()` — Entries awaiting posting
- `save(journalEntry)` — Persist entry

### EmployeeRepository

- `findByEmployeeId(id)` — Retrieve specific employee
- `findByDepartment(departmentCode)` — Employees in a department
- `findActiveEmployees()` — All currently employed staff
- `save(employee)` — Persist employee record

## Design Guidelines

- One repository per aggregate root
- Repository interface in domain layer; implementation in infrastructure
- Return aggregate roots, not raw data
- Use domain-meaningful query methods, not generic `findAll()` with filters

## Relationships to Other Topics

- [Aggregates](../aggregates/) — One repository per aggregate root
- [Entities](../entities/) — Repositories persist aggregate root entities
- [Domain Services](../domain-services/) — Services use repositories to load aggregates

## References

- Evans, Eric. "Domain-Driven Design." Chapter 6. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 12. Addison-Wesley, 2013.
