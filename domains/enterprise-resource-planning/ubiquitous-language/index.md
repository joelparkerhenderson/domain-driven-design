# Enterprise Resource Planning DDD - Ubiquitous Language

Account: A financial record in the general ledger that tracks debits and credits for a specific category such as assets, liabilities, equity, revenue, or expenses.
Accounts Payable: Money owed by the organization to suppliers for goods or services received but not yet paid for.
Accounts Receivable: Money owed to the organization by customers for goods or services delivered but not yet paid for.
Aggregate: A cluster of domain objects treated as a single unit for data consistency, such as a SalesOrder with its LineItems.
Aggregate Root: The single entity through which all access to an aggregate is managed, ensuring invariant enforcement.
Anti-Corruption Layer: A translation boundary that protects a bounded context's domain model from external system concepts.
Backorder: An order for a product that is temporarily out of stock, to be fulfilled when inventory is replenished.
Bill of Materials (BOM): A comprehensive list of raw materials, components, and assemblies required to manufacture a product.
Bounded Context: A well-defined boundary within which a particular domain model applies and terms have specific meaning.
Chart of Accounts: The organized listing of all accounts used by the general ledger to classify financial transactions.
Context Map: A visual and conceptual representation of the relationships and interactions between bounded contexts.
Core Domain: The most critical and complex subdomain that provides competitive advantage and requires the most investment.
Cost Center: An organizational unit or department that incurs costs but does not directly generate revenue.
Credit Memo: A document issued to a customer reducing the amount owed, typically due to returns or pricing adjustments.
Customer: An entity that purchases goods or services from the organization, identified by a unique customer number.
Delivery Note: A document accompanying shipped goods that lists the items, quantities, and delivery details.
Domain Event: A record of something significant that happened in the domain, such as OrderPlaced or PaymentReceived.
Domain Service: An operation that belongs to the domain but does not naturally fit within a single entity or value object.
Employee: A person employed by the organization, identified by an employee ID, with associated payroll and benefits data.
Entity: A domain object defined by its unique identity that persists over time, such as Customer, Product, or Employee.
Fiscal Period: A defined accounting time period (month, quarter, year) used for financial reporting and closing.
Fulfillment: The process of picking, packing, and shipping goods to complete a customer order.
General Ledger: The central accounting record that consolidates all financial transactions across the organization.
Generic Subdomain: A subdomain that is not specific to the organization and can use off-the-shelf solutions.
Goods Receipt: The process of receiving and recording incoming materials or products into the warehouse.
Integration Pattern: A design approach for managing data exchange and communication between bounded contexts.
Inventory: The stock of goods, materials, and products held by the organization in warehouses or transit.
Invoice: A formal document requesting payment for goods or services delivered, listing items, quantities, and amounts.
Journal Entry: A record of a financial transaction in the general ledger with corresponding debit and credit amounts.
Lead Time: The time between placing an order with a supplier and receiving the goods.
Line Item: An individual product or service entry within an order, specifying quantity, unit price, and total.
Material Requirements Planning (MRP): A production planning system that calculates material needs based on demand forecasts and current inventory.
Money: An immutable value representing a monetary amount with its currency code.
Order: A formal request to purchase or sell goods or services, serving as the primary transactional document.
Payroll: The process of calculating and distributing employee compensation including wages, taxes, and deductions.
Pick List: A document directing warehouse staff to collect specific items from storage locations for order fulfillment.
Product: A good or service offered by the organization, identified by a SKU or product number.
Published Language: A well-documented shared data format used for communication between bounded contexts.
Purchase Order: A formal document issued to a supplier authorizing the purchase of specified goods or services.
Purchase Requisition: An internal request to procure goods or services, typically requiring approval before becoming a purchase order.
Quantity: An immutable value representing an amount with its unit of measure.
Repository: A mechanism for encapsulating storage, retrieval, and search behavior for aggregate roots.
Return Merchandise Authorization (RMA): A formal process for customers to return goods, including authorization and tracking.
Revenue: Income generated from the sale of goods or services in the normal course of business.
Sales Order: A formal agreement to deliver goods or services to a customer at specified terms and prices.
Shared Kernel: A small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together.
SKU (Stock Keeping Unit): A unique identifier assigned to a product for inventory tracking and management.
Supplier: An external entity that provides goods or services to the organization, identified by a unique supplier number.
Supporting Subdomain: A subdomain that is necessary for the business but not a core differentiator.
Tax Rate: An immutable value representing the applicable tax percentage for a jurisdiction and product category.
Transfer Order: A document authorizing the movement of inventory between warehouses or storage locations.
Value Object: An immutable domain object defined by its attributes rather than identity, such as Money, Address, or Quantity.
Warehouse: A physical location where inventory is stored, managed, and distributed.
Work Order: A document authorizing the manufacture of a specific quantity of a product according to its bill of materials.
