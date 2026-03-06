# Neurology Domain - DDD Documentation

## Overview

This domain applies Domain-Driven Design (DDD) to neurology systems, encompassing clinical assessment, neuroimaging, neurodegenerative disease management, epilepsy management, neuromuscular disorders, and neurorehabilitation.

## Bounded Contexts

1. Clinical Assessment Context - neurological examination, history taking, cognitive screening, reflex testing
2. Neuroimaging Context - MRI brain/spine, CT, PET, EEG, EMG/NCS interpretation
3. Neurodegenerative Disease Context - Alzheimer's, Parkinson's, ALS, MS disease management
4. Epilepsy Management Context - seizure classification, AED management, surgical evaluation, VNS
5. Neuromuscular Disorders Context - myopathy, neuropathy, NMJ disorders, genetic testing
6. Neurorehabilitation Context - stroke recovery, TBI rehabilitation, cognitive rehabilitation, adaptive tech

## Topics

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
- clinical-assessment-context
- neuroimaging-context
- neurodegenerative-disease-context
- epilepsy-management-context
- neuromuscular-disorders-context
- neurorehabilitation-context
- event-driven-architecture
- integration-patterns
- neurology-standards
- regulatory-compliance

## Conventions

- Each topic resides in its own directory under `topics/` with an `index.md` file.
- Ubiquitous language terms are defined in `ubiquitous-language.md`.
- Business logic is pure and independent of infrastructure concerns.
- Documentation follows DDD strategic and tactical design patterns.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
