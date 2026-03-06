# Subdomains in Fulfillment

## Overview

Subdomains represent the natural divisions of a business domain based on organizational structure and business capability. Domain-Driven Design classifies subdomains into three categories -- core, supporting, and generic -- based on their strategic importance, competitive advantage, and complexity. This classification guides investment decisions: where to apply the best talent, where to use simpler approaches, and where to buy off-the-shelf solutions.

## Relevance to Fulfillment

Fulfillment operations contain a mix of highly specialized, competitively differentiating capabilities alongside routine operational functions and commodity infrastructure. Correctly classifying subdomains ensures that:

- Engineering investment focuses on the areas that drive competitive advantage (faster delivery, lower costs, higher accuracy)
- Supporting capabilities receive adequate but proportional attention
- Generic capabilities are handled by proven third-party solutions rather than custom development

## Core Subdomains

Core subdomains are the source of competitive advantage. They contain the most complex business logic, require deep domain expertise, and justify the highest investment in engineering talent and custom development.

### Warehouse Optimization

- **Pick path optimization**: Algorithms that minimize picker travel distance by sequencing picks across zones and aisles
- **Wave planning**: Grouping orders into pick waves based on carrier cutoff times, service levels, zone efficiency, and labor availability
- **Slotting optimization**: Determining optimal bin locations for products based on pick frequency, product dimensions, and ergonomics
- **Labor allocation**: Assigning warehouse staff to tasks based on workload, skill, and priority in real time

**Why core**: Warehouse efficiency directly determines fulfillment cost per order and throughput capacity. A 10% improvement in pick path efficiency can translate to millions in annual savings for high-volume operations.

### Route and Carrier Optimization

- **Carrier selection algorithms**: Choosing the optimal carrier and service level based on cost, transit time, reliability, and capacity constraints
- **Rate shopping engine**: Real-time comparison of rates across carriers, accounting for dimensional weight pricing, surcharges, and volume discounts
- **Delivery date prediction**: Accurately estimating delivery dates based on carrier performance data, weather, and seasonal patterns
- **Multi-carrier strategy**: Balancing volume across carriers to maintain rate competitiveness and avoid over-reliance on a single carrier

**Why core**: Shipping costs typically represent the largest variable cost in fulfillment. Optimizing carrier selection can reduce shipping costs by 15-25% while maintaining or improving delivery speed.

### Real-Time Inventory Allocation

- **Multi-warehouse allocation**: Determining the optimal fulfillment center for each order based on proximity to destination, inventory availability, and warehouse capacity
- **Intelligent splitting**: Deciding when to split an order across warehouses versus waiting for consolidated inventory
- **Promise-date-aware allocation**: Allocating inventory to meet committed delivery dates while minimizing expedited shipping

**Why core**: Allocation decisions directly impact both shipping cost and customer experience. Poor allocation leads to unnecessary splits, expedited shipping, and missed delivery promises.

## Supporting Subdomains

Supporting subdomains are necessary for the business to function but do not provide competitive differentiation. They have moderate complexity and can often use established patterns or slightly customized solutions.

### Order Processing

- Order intake and validation from multiple channels
- Order splitting and routing logic
- Order status management and customer communication
- Hold and cancellation workflows

**Why supporting**: While essential, order processing follows well-established patterns. The competitive advantage lies not in how orders are received but in how efficiently they are fulfilled.

### Inventory Management

- Stock level tracking and accuracy maintenance
- Cycle counting and reconciliation
- Backorder management
- Safety stock and reorder point calculation

**Why supporting**: Inventory accuracy is critical but the underlying patterns are well-understood. The value comes from integration with the core allocation and warehouse optimization subdomains.

### Returns Processing

- Return merchandise authorization
- Return receiving and inspection
- Refund and exchange processing
- Returned goods disposition

**Why supporting**: Returns processing follows established workflows. While important for customer satisfaction, it rarely differentiates fulfillment operations competitively.

### Delivery Tracking

- Carrier tracking event aggregation
- Unified status normalization
- Customer notification
- Proof of delivery management

**Why supporting**: Tracking is a table-stakes capability. Customers expect it, but the competitive advantage is small once basic tracking is in place.

## Generic Subdomains

Generic subdomains provide commodity capabilities that are common across many business domains. They should be purchased, adopted from open-source, or implemented with minimal custom effort.

### Authentication and Authorization

- User identity management
- Role-based access control
- API authentication (OAuth, API keys)
- Single sign-on integration

### Notifications

- Email notifications (order confirmation, shipping updates, delivery confirmation)
- SMS alerts
- Push notifications
- Notification template management

### File Storage

- Shipping label storage and retrieval
- Document management (customs forms, packing slips, invoices)
- Image storage (proof of delivery photos, return item photos)

### Logging and Monitoring

- Application logging
- System health monitoring
- Performance metrics collection
- Alerting and escalation

### Reporting and Analytics

- Operational dashboards
- Historical reporting
- Data warehousing
- Business intelligence integration

## Subdomain Classification Decision Framework

When classifying a new capability, consider:

1. **Does it differentiate us competitively?** If yes, it is core.
2. **Would a competitor gain advantage by doing it better?** If yes, it may be core.
3. **Is it specific to fulfillment but follows established patterns?** If yes, it is supporting.
4. **Is it a capability found in most enterprise software?** If yes, it is generic.
5. **Can we buy a solution that meets 80% of our needs?** If yes, it is likely generic or supporting.

## Relationships to Other Topics

- [Strategic Design](../strategic-design/) -- Subdomain classification is a key strategic design activity
- [Bounded Contexts](../bounded-contexts/) -- Subdomains often align with bounded context boundaries
- [Context Map](../context-map/) -- Core subdomains typically own their context; generic subdomains may be shared
- [Domain Services](../domain-services/) -- Core subdomain logic is often implemented as domain services
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Generic subdomains accessed through external services use ACLs

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 15: Distillation. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 2: Domains, Subdomains, and Bounded Contexts. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 1: What Is Domain-Driven Design? O'Reilly, 2021.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Chapter 2: Strategic Design with Subdomains. Addison-Wesley, 2016.
