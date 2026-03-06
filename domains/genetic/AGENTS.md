# Genetics Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to genetics systems, encompassing genomic testing, variant interpretation, genetic counseling, pharmacogenomics, hereditary disease management, and reproductive genetics.

## Bounded Contexts

1. Genomic Testing Context - specimen collection, sequencing workflows, quality control, bioinformatics pipelines, test result generation
2. Variant Interpretation Context - variant classification (ACMG/AMP guidelines), pathogenicity assessment, allele frequency analysis, functional annotation
3. Genetic Counseling Context - patient risk assessment, family pedigree analysis, pre-test and post-test counseling, psychosocial support
4. Pharmacogenomics Context - drug-gene interaction profiling, metabolizer phenotype classification, prescribing guidance, clinical decision support
5. Hereditary Disease Management Context - Mendelian disorder tracking, carrier screening, cascade testing coordination, surveillance protocols
6. Reproductive Genetics Context - preconception carrier screening, prenatal genetic testing, preimplantation genetic testing (PGT), newborn screening

## Domain Principles

- Model using shared ubiquitous language between geneticists, genetic counselors, laboratory scientists, bioinformaticians, and system developers.
- Group the system into bounded contexts reflecting clinical genetics subspecialties and laboratory workflows.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow ACMG/AMP standards for variant classification and clinical reporting terminology.
- Maintain HIPAA compliance, GINA protections, and patient consent as cross-cutting concerns.

## Key Topics

- strategic-design
- bounded-contexts
- context-map
- ubiquitous-language
- subdomains
- anti-corruption-layer
- aggregates
- entities
- value-objects
- domain-events
- repositories
- domain-services
- genomic-testing-context
- variant-interpretation-context
- genetic-counseling-context
- pharmacogenomics-context
- hereditary-disease-management-context
- reproductive-genetics-context
- event-driven-architecture
- integration-patterns
- genetics-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Richards, Sue, et al. Standards and Guidelines for the Interpretation of Sequence Variants. Genetics in Medicine, 2015.
- Nussbaum, Robert L., et al. Thompson & Thompson Genetics in Medicine. Elsevier, 2016.
