# Genetics Domain - Tasks

## Strategic Design

- [ ] Identify core, supporting, and generic subdomains for genetics.
- [ ] Define six bounded contexts aligned with genetics subspecialties and laboratory workflows.
- [ ] Create context map showing relationships between bounded contexts.
- [ ] Establish ubiquitous language with domain experts.
- [ ] Design anti-corruption layers for external system integration (EHR, LIMS, genomic databases).

## Tactical Design

- [ ] Model aggregates for each bounded context.
- [ ] Define entities with unique identities within genetics workflows (test orders, patients, variants).
- [ ] Design value objects for immutable genetics data (genomic coordinates, HGVS nomenclature, zygosity).
- [ ] Identify domain events that cross bounded context boundaries (VariantClassified, TestCompleted, CounselingScheduled).
- [ ] Define repository abstractions for aggregate persistence.
- [ ] Design domain services for cross-aggregate operations such as cascade testing coordination.

## Documentation

- [ ] Document Genomic Testing Context with sequencing workflow and result reporting models.
- [ ] Document Variant Interpretation Context with ACMG/AMP classification models.
- [ ] Document Genetic Counseling Context with pedigree analysis and risk assessment models.
- [ ] Document Pharmacogenomics Context with drug-gene interaction and metabolizer phenotype models.
- [ ] Document Hereditary Disease Management Context with carrier screening and surveillance models.
