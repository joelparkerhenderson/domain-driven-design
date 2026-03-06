# Enterprise Resource Planning - Domain-Driven Design

This domain applies Domain-Driven Design (DDD) to Enterprise Resource Planning (ERP), creating a modular and scalable system by modeling software directly after complex business operations using a shared "ubiquitous language" between developers and business domain experts (accountants, warehouse managers, procurement specialists, HR professionals).

## Bounded Contexts

- **Finance/Accounting Context** - General ledger, accounts payable/receivable, financial reporting
- **Procurement Context** - Purchase orders, supplier management, sourcing
- **Inventory/Warehouse Context** - Stock management, warehouse operations, logistics
- **Sales/Order Context** - Sales orders, customer management, pricing, fulfillment
- **Human Resources Context** - Employee management, payroll, benefits, recruitment
- **Manufacturing Context** - Production planning, bill of materials, work orders, quality control

## Ubiquitous Language

The shared vocabulary between developers and business domain experts is defined in `language.md`.

## Directory Structure

- `index.md` - Main documentation entry point
- `plan.md` - Project plan
- `tasks.md` - Task tracking
- `language.md` - Ubiquitous language glossary
- `topics/` - Detailed topic documentation

## Conventions

- Every directory has `index.md` with a symlink `README.md` -> `index.md`
- No images, diagrams, web apps, front-end, or back-end code
- Comprehensive documentation with references and citations
- Ubiquitous language terms: one per line, one-sentence explanation

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Vernon, Vaughn. "Domain-Driven Design Distilled." Addison-Wesley, 2016.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Wrox, 2015.
- Monk, Ellen & Wagner, Bret. "Concepts in Enterprise Resource Planning." Cengage Learning, 2012.
