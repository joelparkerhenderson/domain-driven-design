# Domain Driven Design (DDD) - Project Instructions

## Overview

This project applies Domain-Driven Design (DDD) to create comprehensive domain models with ubiquitous language, bounded contexts, strategic design, and tactical design patterns.

DDD creates modular scalable systems:

- By modeling using shared "ubiquitous language" between domain experts, domain developers, and domain stakeholders.

- By grouping the system into bounded contexts.

## Project Structure

```
.
├── index.md                  # Project overview and DDD introduction
├── AGENTS.md                 # Project instructions (this file)
├── CLAUDE.md                 # Points to AGENTS.md
├── bin/
│   ├── list-domains          # List all domain directories
│   └── test-domains          # Verify domain directory structure
└── domains/
    ├── AGENTS.md             # Domain-specific instructions and domain list
    └── {domain-name}/        # One directory per domain
        ├── index.md          # Domain introduction and navigation
        ├── README.md         # Symlink → index.md
        ├── AGENTS.md         # Domain agent instructions
        ├── CLAUDE.md         # Points to AGENTS.md
        ├── plan.md           # Domain plan with goals, scope, milestones
        ├── tasks.md          # Domain task tracking
        ├── ubiquitous-language/
        │   ├── index.md      # Terminology list with definitions
        │   └── README.md     # Symlink → index.md
        └── topics/
            └── {topic}/
                ├── index.md  # Topic documentation
                └── README.md # Symlink → index.md
```

## Guides

Permissions:

- Do not read above this project directory.

After each large task:

- Update, upgrade, harmonize, audit.

- Improve CLAUDE.md file.

- Improve index.md file.

Verify this directory:

- index.md file

- symlink real index.md file to link README.md file: `ln -sfn index.md README.md`

Verify each domain directory:

- CLAUDE.md file

- plan.md file

- tasks.md file

- index.md file

- directory "ubiquitous-language"
  - file index.md with a list of each topic: one per line, each with one-sentence explanation
  - symlink from index.md to README.md

- directory "topics"

- Each topic in its own directory with its own index.md file: `./topics/{topic}/index.md` and symlink `./topics/{topic}/README.md`

- Create comprehensive documentation.

- Do not create images, diagrams, web applications, front-end, back-end, etc.

- Provide references, citations, such as whitepapers, books, articles.

## Strategic Design Patterns

Strategic design addresses how to decompose a large system into manageable bounded contexts.

- **Bounded Contexts**: Logical boundaries where specific models and terminologies are consistent.

- **Context Map**: Visual representation of relationships and interactions between bounded contexts.

- **Subdomains**: Classification of domain areas by strategic importance (core, supporting, generic).

- **Anti-Corruption Layer**: Translation boundary that protects a bounded context from external system concepts.

## Tactical Design Patterns

Tactical patterns define how the business logic is structured within bounded contexts.

- **Entities**: Objects with a unique identity which persist over time even as their attributes change.

- **Value Objects**: Immutable objects defined by their attributes rather than identity.

- **Aggregates**: Clusters of related objects treated as a single unit that maintains consistency across its tasks and resources.

- **Domain Events**: Significant occurrences within the domain which can trigger actions in other bounded contexts.

- **Repositories**: Abstractions for storing and retrieving aggregate roots.

- **Domain Services**: Stateless operations that span multiple aggregates.

## Cross-Cutting Concerns

- **Event-Driven Architecture**: Communication between bounded contexts through domain events.

- **Integration Patterns**: Shared kernel, published language, open host service, customer-supplier.

- **Standards**: Domain-specific standards that inform value object design and published languages.

- **Regulatory Compliance**: Legal and governance requirements that shape domain model design.

- **Business Logic**: Pure and independent. Do not use any infrastructure details such as database or UI or web or mobile.

- **CQRS** (Command Query Responsibility Segregation): Separate read-only logic, write-only logic, and read-write logic.

## References

- Evans, Eric. _Domain-Driven Design: Tackling Complexity in the Heart of Software_. Addison-Wesley, 2003.
- Vernon, Vaughn. _Implementing Domain-Driven Design_. Addison-Wesley, 2013.
- Khononov, Vlad. _Learning Domain-Driven Design_. O'Reilly Media, 2021.

## Verify

Run $(bin/test)
