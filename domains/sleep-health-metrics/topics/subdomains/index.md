# Subdomains - Sleep Health Metrics

## Overview

Subdomain classification determines how strategic resources are allocated across the sleep health metrics domain. Domain-Driven Design classifies subdomains into three categories -- core, supporting, and generic -- based on their strategic importance and competitive differentiation potential. In sleep health metrics, this classification guides where to invest the most domain expertise and modeling rigor versus where to adopt standard solutions.

## Core Subdomains

Core subdomains contain the most complex, valuable, and differentiating domain logic. They represent the areas where deep sleep medicine expertise creates the highest value and where a generic solution would be inadequate.

### Sleep Stage Analysis

Sleep stage analysis is a core subdomain because accurate epoch-by-epoch scoring directly determines the quality and clinical utility of all downstream metrics. The complexity of AASM scoring rules -- involving EEG frequency analysis, K-complex and sleep spindle detection, rapid eye movement identification, and inter-scorer reliability considerations -- demands deep domain modeling. Differentiation comes from the sophistication of the scoring model: how it handles ambiguous epochs, artifact-contaminated segments, and transitions between stages.

### Sleep Quality Assessment

Sleep quality assessment is a core subdomain because the computation of clinically meaningful metrics from staged data requires nuanced domain knowledge. Simple calculations like sleep efficiency are straightforward, but composite indices (PSQI, ESS), longitudinal trend detection, normative age-adjusted comparisons, and multi-night statistical summaries require sophisticated modeling. The ability to produce actionable clinical insights from raw metrics is the domain's primary value proposition.

## Supporting Subdomains

Supporting subdomains provide essential capability but follow more established patterns. They require domain expertise but do not represent the primary competitive differentiator.

### Sleep Data Collection

Data collection is a supporting subdomain. While essential for the entire pipeline, the processes of signal acquisition, channel validation, and data formatting follow well-established polysomnography and actigraphy standards. The complexity lies in handling diverse data sources (PSG systems, wearables, sleep diaries) but the underlying collection logic is not where the domain creates its primary value.

### Circadian Rhythm Tracking

Circadian rhythm tracking is a supporting subdomain. Chronotype assessment, light exposure dosimetry, and DLMO estimation follow established chronobiology methods. The models are well-defined in research literature. The value this subdomain adds is in contextualizing sleep quality with circadian phase data, but the circadian models themselves are not novel to this domain.

### Intervention Tracking

Intervention tracking is a supporting subdomain. CBT-I protocols have standardized modules (stimulus control, sleep restriction, cognitive restructuring). CPAP adherence monitoring follows CMS and insurance compliance definitions (4 hours per night, 70 percent of nights). Medication tracking follows pharmacological standards. The complexity is in integrating multiple intervention modalities and correlating them with outcomes, but the individual intervention models are well-established.

## Generic Subdomains

Generic subdomains can be addressed with off-the-shelf solutions or standard patterns. They are necessary but do not require deep domain expertise to implement.

### Clinical Integration

Clinical integration is a generic subdomain. FHIR R4 resource mapping, HL7 messaging, EHR API connectivity, and clinical document generation follow published standards with existing reference implementations. The implementation may be technically challenging, but the domain logic is standardized rather than proprietary. A FHIR observation resource for AHI follows the same structural rules regardless of which sleep health system produces it.

## Classification Rationale

The classification follows the principle that core subdomains should receive the most rigorous domain modeling and the most experienced domain experts. Supporting subdomains benefit from solid modeling but can leverage established patterns. Generic subdomains should adopt standard solutions to avoid unnecessary investment.

This classification informs decisions about:

- Where to allocate sleep medicine domain experts for model validation.
- Where to invest in custom algorithm development versus adopting existing libraries.
- Where to apply rigorous aggregate design versus simpler CRUD-like patterns.
- Where to build versus buy or integrate with existing systems.

## Subdomain Interactions

Core subdomains (Sleep Stage Analysis, Sleep Quality Assessment) form the analytical heart of the domain. They consume data from the supporting subdomain (Sleep Data Collection), are enriched by another supporting subdomain (Circadian Rhythm), inform a third supporting subdomain (Intervention Tracking), and publish results through the generic subdomain (Clinical Integration).

## Evolution

Subdomain classification is not permanent. As automated sleep scoring algorithms become commoditized, Sleep Stage Analysis might shift from core to supporting. Conversely, if advanced circadian phenotyping becomes a differentiator, the Circadian Rhythm subdomain might be promoted to core. Regular reassessment ensures resource allocation remains aligned with strategic value.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 15: Distillation.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 2: Subdomains.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 3: Managing Domain Complexity.
