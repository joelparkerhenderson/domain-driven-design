# Procurement Context in Enterprise Resource Planning

## Overview

The Procurement Context manages the acquisition of goods and services from external suppliers: purchase requisitions, request for quotations, purchase orders, goods receipt, supplier management, and sourcing strategies.

## Core Responsibilities

- **Requisition Management**: Capturing internal purchase requests and routing them through approval workflows
- **Supplier Management**: Maintaining supplier records, evaluating performance, managing contracts
- **Purchase Order Management**: Creating, approving, and tracking purchase orders
- **Sourcing**: Issuing RFQs, evaluating bids, selecting suppliers
- **Goods Receipt**: Confirming delivery of ordered goods against purchase orders
- **Contract Management**: Managing long-term agreements, blanket orders, and framework contracts

## Aggregates

### PurchaseOrder Aggregate

- **Root**: PurchaseOrder (identified by PO Number)
- **Internal entities**: PurchaseLineItem, GoodsReceiptRecord
- **Value objects**: Money, Quantity, DeliveryTerms, PaymentTerms, ShippingAddress
- **States**: Draft → Submitted → Approved → Sent → Partially Received → Fully Received → Closed
- **Invariants**: PO total within approval authority; approved POs require re-approval for material changes; goods receipt quantity cannot exceed ordered quantity (without tolerance override)

### Supplier Aggregate

- **Root**: Supplier (identified by Supplier Number)
- **Value objects**: Address, PaymentTerms, QualityRating, ContactInfo
- **Invariants**: Blocked suppliers cannot receive new POs; supplier rating calculated from delivery and quality history

### PurchaseRequisition Aggregate

- **Root**: PurchaseRequisition (identified by Requisition Number)
- **Internal entities**: RequisitionLineItem
- **Value objects**: Money, Quantity, RequestedDeliveryDate
- **States**: Draft → Submitted → Approved → Converted to PO → Closed

## Domain Events

| Event | Trigger | Consumers |
|-------|---------|-----------|
| RequisitionCreated | Internal purchase request | Approval workflow |
| PurchaseOrderApproved | PO authorized | Supplier (notification), Finance (commitment) |
| PurchaseOrderSent | PO transmitted to supplier | Tracking |
| GoodsReceived | Delivery accepted | Inventory (stock update), Finance (accrual), Quality |
| SupplierInvoiceReceived | Invoice from supplier | Finance (AP posting) |
| SupplierEvaluated | Performance review completed | Supplier record update |

## Integration with Other Contexts

- **Manufacturing → Procurement**: MRP generates purchase requisitions for material shortages
- **Inventory → Procurement**: StockBelowReorderPoint triggers automatic requisitions
- **Procurement → Finance**: PO approval creates financial commitment; goods receipt triggers accrual
- **Procurement → Inventory**: GoodsReceived updates stock levels

## Key Workflows

### Procure-to-Pay

1. Need identified → Requisition created
2. Requisition approved → Purchase order generated
3. PO sent to supplier → Supplier confirms
4. Goods delivered → Receipt recorded (3-way match: PO, receipt, invoice)
5. Supplier invoice received → AP posted
6. Payment processed → Supplier paid

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) — One of six ERP bounded contexts
- [Context Map](../context-map/) — Upstream to Finance and Inventory, downstream from Manufacturing
- [Aggregates](../aggregates/) — PurchaseOrder, Supplier, PurchaseRequisition are aggregate roots
- [Domain Events](../domain-events/) — Procurement events drive Finance and Inventory
- [Anti-Corruption Layer](../anti-corruption-layer/) — ACL for supplier portal integrations

## References

- Evans, Eric. "Domain-Driven Design." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Monczka, Handfield, Giunipero, & Patterson. "Purchasing and Supply Chain Management." Cengage Learning, 2015.
