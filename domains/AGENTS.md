# Domains - Agent Instructions

## Overview

This directory contains domain models that apply Domain-Driven Design (DDD) principles. Each domain directory follows a consistent structure with ubiquitous language, bounded contexts, strategic design, and tactical design documentation.

## Domain Directory Structure

Each domain directory must contain:

- `index.md` - Domain introduction, bounded contexts, topics list, references
- `README.md` - Symlink to index.md
- `AGENTS.md` - Domain-specific agent instructions and principles
- `CLAUDE.md` - Points to AGENTS.md
- `plan.md` - Goals, scope, approach, milestones
- `tasks.md` - Task tracking with checkboxes
- `ubiquitous-language/` - Directory with terminology
  - `index.md` - List of terms, one per line, each with one-sentence explanation
  - `README.md` - Symlink to index.md
- `topics/` - Directory with topic subdirectories
  - `{topic}/index.md` - Topic documentation
  - `{topic}/README.md` - Symlink to index.md

## Guides

- Create comprehensive documentation.
- Do not create images, diagrams, web applications, front-end, back-end, etc.
- Provide references, citations, such as whitepapers, books, articles.
- Business logic remains pure and independent of infrastructure (database, UI, web, mobile).
- Model using shared ubiquitous language between domain experts, developers, and stakeholders.
- Group the system into bounded contexts reflecting real-world specialties and workflows.

## List of Domains

- [advance decision to refuse treatment](advance-decision-to-refuse-treatment)
- [advance statement about care](advance-statement-about-care)
- [agile](agile)
- [allergy](allergy)
- [asthma](asthma)
- [attention deficit](attention-deficit)
- [audiology](audiology)
- [autism](autism)
- [body health metrics](body-health-metrics)
- [cardiology](cardiology)
- [cognitive science](cognitive-science)
- [computer science](computer-science)
- [consent to treatment](consent-to-treatment)
- [contraception](contraception)
- [dental](dental)
- [dentistry](dentistry)
- [dermatology](dermatology)
- [education sector](education-sector)
- [energy sector](energy-sector)
- [enterprise resource planning](enterprise-resource-planning)
- [ergonomics](ergonomoic)
- [finance sector](finance-sector)
- [fulfillment](fulfillment)
- [gastroenterology](gastroenterology)
- [genetics](genetic)
- [gerontology](gerontology)
- [gynecology](gynecology)
- [hearing aid](hearing-aid)
- [heart health metrics](heart-health-metrics)
- [hematology](hematology)
- [hormone replacement therapy](hormone-replacement-therapy)
- [hospital management](hospital-management)
- [kinesiology](kinesiology)
- [learning management system](learning-management-system)
- [legal sector](legal-sector)
- [logistics](logistics)
- [mast cell activation syndrome](mast-cell-activation-syndrome)
- [medical records release permission](medical-records-release-permission)
- [medical sector](medical-sector)
- [mental health](mental-health)
- [mobility](mobility)
- [neurology](neurology)
- [nutrition](nutrition)
- [occupational therapy](occupational-therapy)
- [okr](okr)
- [oncology](oncology)
- [ophthalmology](opthamology)
- [orthopaedics](orthopaedics)
- [otolaryngology](otolaryngology)
- [pediatrics](pediatrics)
- [pre-operative assessment](pre-operative-assessment)
- [prenatal](prenatal)
- [project portfolio management](project-portfolio-management)
- [psychiatry](psychiatry)
- [psychology](psychology)
- [pulmonology](pulmonolgy)
- [respirology](respirology)
- [retail](retail)
- [rheumatology](rheumatology)
- [sales](sales)
- [semaglutide](semaglutide)
- [sleep health metrics](sleep-health-metrics)
- [sleep quality](sleep-quality)
- [stroke](stroke)
- [transport sector](transport-sector)
- [travel](travel)
- [urology](urology)
- [vaccinations](vaccinations)

## Verify

Run `bin/test-domains`
