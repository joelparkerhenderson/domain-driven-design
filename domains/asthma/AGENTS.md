# Asthma Domain - DDD Project Instructions

## Overview

This domain applies Domain-Driven Design (DDD) to asthma management systems. The domain covers clinical assessment, treatment management, trigger monitoring, action planning, medication management, and outcomes tracking.

## Bounded Contexts

1. Clinical Assessment Context - spirometry, peak flow, FeNO testing, severity classification (GINA)
2. Treatment Management Context - stepwise therapy, biologic agents, immunotherapy, inhaler technique
3. Trigger Monitoring Context - allergen tracking, air quality, environmental factors, occupational exposures
4. Action Planning Context - asthma action plans, zone system (green/yellow/red), emergency protocols
5. Medication Management Context - controller medications, rescue inhalers, adherence monitoring
6. Outcomes Tracking Context - ACT scores, exacerbation rates, lung function trends, quality of life

## Domain Principles

- Model asthma management using ubiquitous language shared by clinicians, respiratory therapists, patients, and developers.
- Maintain clear bounded context boundaries between clinical assessment, treatment, and monitoring concerns.
- Ensure regulatory compliance with HIPAA, clinical guidelines (GINA, NAEPP), and pharmaceutical standards.
- Business logic must remain pure and independent of infrastructure details such as databases, UI, or web/mobile platforms.

## Topics

Each topic resides in its own directory under `topics/` with an `index.md` file.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Global Initiative for Asthma (GINA). Global Strategy for Asthma Management and Prevention, 2023.
- National Asthma Education and Prevention Program (NAEPP). Expert Panel Report 4, 2020.
