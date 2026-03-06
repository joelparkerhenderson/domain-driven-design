# Kinesiology Domain - CLAUDE.md

## Overview

This domain applies Domain-Driven Design (DDD) to kinesiology systems, encompassing movement assessment, exercise prescription, rehabilitation planning, performance analysis, injury prevention, and research/education.

## Bounded Contexts

1. Movement Assessment Context - gait analysis, biomechanical evaluation, range of motion, functional testing
2. Exercise Prescription Context - program design, periodization, progressive overload, modality selection
3. Rehabilitation Planning Context - post-injury protocols, return-to-play criteria, therapeutic exercise
4. Performance Analysis Context - force plate analysis, motion capture, sport-specific metrics
5. Injury Prevention Context - screening protocols, movement correction, load management, prehabilitation
6. Research & Education Context - evidence-based practice, curriculum development, continuing education

## Domain Principles

- Model movement science using ubiquitous language shared between kinesiologists, clinicians, coaches, and researchers.
- Maintain clear bounded context boundaries between assessment, prescription, rehabilitation, performance, prevention, and education.
- Use domain events to communicate significant occurrences across bounded contexts.
- Keep business logic pure and independent of infrastructure concerns.
- Respect kinesiology-specific standards (ACSM, NSCA, NATA) and regulatory compliance requirements.

## File Structure

- CLAUDE.md - this file
- plan.md - strategic plan for the domain
- tasks.md - task tracking
- index.md - main documentation index
- ubiquitous-language.md - shared terminology
- topics/ - detailed topic documentation

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
