# Sales - Domain-Driven Design (DDD)

The "Sales" domain is a classic example used to illustrate how complex business logic is separated into manageable Bounded Contexts.

## Strategic Design: Mapping the Sales Domain

DDD splits the problem space into Subdomains:

- Lead Generation (Supportive): Focuses on potential customers ("Prospects") and marketing campaigns.
- Sales/Opportunity Management (Core): The heart of the business where deals are negotiated and closed.
- Order Fulfillment (Generic): Handles the logistics after a sale is made.

## Bounded Contexts & Ubiquitous Language

Within each context, terms like "Customer" take on distinct meanings:

- Sales Context: A "Customer" is a Prospect with a budget and an assigned sales rep.
- Billing Context: A "Customer" is an Account with a credit limit and tax ID.
- Shipping Context: A "Customer" is a Recipient with a physical address and delivery instructions.

## Tactical Patterns for Sales

These patterns implement the business rules inside a Bounded Context:

- Aggregates: A cluster of objects treated as a single unit. Example: An Order aggregate root might contain multiple OrderLines (Entities) and a ShippingAddress (Value Object). All changes to line items must go through the Order root to ensure the total price remains consistent.
- Entities: Objects with a unique identity that persists over time, such as a SalesOrder or a Contract.
- Value Objects: Descriptors with no identity, like Currency, Price, or DiscountPercentage.
- Domain Events: Significant business occurrences, such as OrderPlaced, PaymentReceived, or LeadQualified.

## Integration via Context Mapping

Since Sales must talk to Billing and Shipping, you use a Context Map to define the relationships:

- Customer/Supplier: Shipping (Customer) depends on the data provided by Sales (Supplier).
- Shared Kernel: A subset of the model (like a shared Account ID) used by both Sales and Finance.
- Anti-Corruption Layer (ACL): A translation layer that prevents the messy "Sales" model from polluting a clean new "CRM" system.
