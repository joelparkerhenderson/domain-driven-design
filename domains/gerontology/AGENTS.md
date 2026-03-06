# Gerontology Domain - CLAUDE.md

## Domain Overview

This domain applies Domain-Driven Design (DDD) to gerontology and geriatric care systems. Gerontology is the study of aging and the challenges associated with older adults, while geriatric care focuses on the clinical, social, and functional needs of aging populations. The domain models geriatric assessment, care coordination, cognitive health, functional independence, medication management, and end-of-life planning.

## Bounded Contexts

1. **Geriatric Assessment Context** - Comprehensive geriatric assessment (CGA), frailty screening, fall risk evaluation, and multidimensional health status determination.

2. **Care Coordination Context** - Interdisciplinary team management, care transitions, caregiver support, and community resource linkage.

3. **Cognitive Health Context** - Dementia screening, cognitive testing (MMSE, MoCA), memory care planning, and cognitive decline tracking.

4. **Functional Independence Context** - ADL/IADL assessment, mobility aids, home modification planning, and assistive technology management.

5. **Medication Management Context** - Polypharmacy review, Beers criteria application, deprescribing protocols, and medication adherence monitoring.

6. **End of Life Planning Context** - Advance directives, palliative care coordination, hospice referral, and goals-of-care conversations.

## Key Principles

- Model using shared ubiquitous language between geriatricians, nurses, social workers, caregivers, and developers.
- Separate read-only, write-only, and read-write logic (CQRS).
- Business logic remains pure and independent of infrastructure.
- Domain events drive communication between bounded contexts.
- Regulatory compliance (HIPAA, CMS, state elder care regulations) shapes model design.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software.
- Vernon, V. (2013). Implementing Domain-Driven Design.
- Khononov, V. (2021). Learning Domain-Driven Design.
- Rubenstein, L.Z. et al. (1991). Comprehensive Geriatric Assessment.
- American Geriatrics Society (AGS). Beers Criteria for Potentially Inappropriate Medication Use in Older Adults.
