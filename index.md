# Domain Driven Design (DDD)

Domain-Driven Design (DDD) is a software development philosophy and set of practices that prioritize the business domain—the specific area of knowledge or activity the software serves—at the core of all design decisions. Coined by Eric Evans in his 2003 book, *Domain-Driven Design: Tackling Complexity in the Heart of Software*, it aims to align technical implementation closely with real-world business processes.

DDD is generally divided into two main areas of focus: Strategic and Tactical design.

## Strategic Design (The Big Picture)

- **Ubiquitous Language**: A shared, common vocabulary used by both developers and domain experts (stakeholders) in conversation, documentation, and directly in the code (e.g., class and method names).

- **Bounded Context**: A logical boundary within a system where a specific domain model and its language are consistent and valid.

- **Context Mapping**: Diagrams and patterns (like Anti-Corruption Layers or Shared Kernels) that define how different bounded contexts interact and relate to one another.

## Tactical Design (Technical Implementation)

- **Entities**: Objects with a unique, enduring identity that persists even if their attributes change (e.g., a "Customer" with a unique ID).

- **Value Objects**: Immutable objects defined only by their attributes rather than a unique identity (e.g., an "Address" or "Money" type).

- **Aggregates**: Clusters of related entities and value objects treated as a single unit for data consistency, managed by a designated Aggregate Root.

- **Domain Events**: Significant occurrences within the business domain that other parts of the system may need to react to (e.g., "OrderPlaced").

- **Repositories**: Abstractions that handle the retrieval and persistence of domain objects, hiding the technical details of the data store from the domain layer.

- **Domain Services**: Stateless operations that encapsulate business logic spanning multiple aggregates.

## Cross-Cutting Concerns

- **Event-Driven Architecture**: Communication between bounded contexts through domain events.

- **Integration Patterns**: Shared kernel, published language, open host service, customer-supplier.

- **CQRS**: Command Query Responsibility Segregation separates read and write models.

- **Standards**: Domain-specific standards that inform value object design and published languages.

- **Regulatory Compliance**: Legal and governance requirements that shape domain model design.

## Domains

See [domains](domains/) for all domain models in this project.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
