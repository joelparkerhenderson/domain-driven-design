# Education Sector Domain - AGENTS.md

## Overview

This domain applies Domain-Driven Design (DDD) to education sector systems, encompassing curriculum design and management, student enrollment and administration, academic assessment and grading, learning delivery and instruction, accreditation and quality assurance, and student support services.

## Bounded Contexts

1. Curriculum Management - Manages the design, versioning, approval, and publication of curricula, courses, modules, learning outcomes, and credit frameworks across academic programs.
2. Student Administration - Handles student enrollment, registration, academic records, progression tracking, graduation requirements, and transcript generation.
3. Assessment and Grading - Governs the creation, administration, marking, moderation, and reporting of formative and summative assessments, including grade calculations and academic standing determination.
4. Learning Delivery - Tracks the scheduling, delivery, and resource allocation for instructional activities including lectures, seminars, labs, tutorials, and online learning sessions.

## Domain Principles

- Model using shared ubiquitous language between educators, academic administrators, registrars, students, accreditation bodies, and system developers.
- Group the system into bounded contexts reflecting the distinct workflows of curriculum governance, student lifecycle management, assessment, and instructional delivery.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Follow national qualification frameworks, accreditation body standards, and data protection regulations such as FERPA (US) and GDPR (EU/UK).

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
- curriculum-management
- student-administration
- assessment-and-grading
- learning-delivery
- event-driven-architecture
- integration-patterns
- education-standards
- regulatory-compliance

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- IMS Global Learning Consortium. Learning Tools Interoperability (LTI) Specification. IMS Global, various years.
- European Commission. European Qualifications Framework (EQF). European Commission, 2017.
- Anderson, Lorin W., and David R. Krathwohl, eds. A Taxonomy for Learning, Teaching, and Assessing: A Revision of Bloom's Taxonomy. Longman, 2001.
