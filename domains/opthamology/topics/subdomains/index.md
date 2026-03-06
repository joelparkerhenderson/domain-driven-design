# Subdomains in Ophthalmology

## Overview

Subdomains classify areas of a domain by their strategic importance to the organization.
Domain-Driven Design distinguishes three types of subdomains: core, supporting, and
generic. In the ophthalmology domain, this classification guides investment decisions,
development priorities, and build-versus-buy strategies for each area of the system.

## Subdomain Classification

### Core Subdomains

Core subdomains represent the areas where the ophthalmology practice derives its
primary value and competitive advantage. These require the deepest domain expertise,
the most careful modeling, and the highest investment in development.

**Clinical Examination** is a core subdomain because the quality and accuracy of
clinical examination directly determines patient outcomes and clinical reputation.
The examination workflow, diagnostic reasoning, and clinical decision-making processes
are complex, nuanced, and deeply tied to ophthalmologic expertise. A generic solution
cannot adequately capture the clinical judgment involved in a comprehensive eye
examination.

**Surgical Management** is a core subdomain because operative planning, technique
selection, implant choice, and perioperative management require sophisticated clinical
knowledge. The complexity of modeling surgical workflows, risk assessment, and outcome
tracking across multiple procedure types (cataract, vitrectomy, corneal transplant,
oculoplastics) demands custom domain modeling.

### Supporting Subdomains

Supporting subdomains provide essential capabilities that enhance the core but are
more standardized in nature. They require domain expertise but follow more established
patterns.

**Diagnostic Imaging** is a supporting subdomain. While imaging is critical to clinical
decision-making, imaging acquisition and interpretation follow standardized protocols
(DICOM, structured reporting). The value lies in accurate integration with clinical
workflows rather than in novel imaging concepts.

**Glaucoma Management** is a supporting subdomain. Glaucoma management follows
evidence-based guidelines (American Academy of Ophthalmology Preferred Practice
Patterns) and involves well-defined protocols for IOP monitoring, visual field
progression analysis, and treatment escalation. The complexity lies in longitudinal
data analysis rather than novel clinical concepts.

**Retinal Care** is a supporting subdomain. Retinal conditions and their management
follow established classification systems (ETDRS for diabetic retinopathy, AREDS for
AMD) and treatment protocols (anti-VEGF injection schedules). The models are complex
but well-defined by clinical evidence.

### Generic Subdomains

Generic subdomains follow well-established, standardized approaches that do not provide
strategic differentiation. These are candidates for commercial off-the-shelf solutions
or simplified implementations.

**Refractive Services** is a generic subdomain. LASIK, PRK, and implantable lens
procedures follow highly standardized protocols with well-defined candidacy criteria,
surgical techniques, and outcome measures. The workflows are mature and widely
replicated across practices with minimal variation.

## Strategic Implications

### Investment Allocation

- Core subdomains receive the highest investment in domain modeling, development
  resources, and ongoing refinement.
- Supporting subdomains receive moderate investment, focusing on integration quality
  and adherence to clinical guidelines.
- Generic subdomains receive minimal custom development, favoring standardized
  approaches or third-party solutions.

### Team Structure

- Core subdomains are staffed with the most experienced developers working closely
  with clinical domain experts.
- Supporting subdomains may use cross-functional teams that serve multiple contexts.
- Generic subdomains may use commodity solutions configured by operations teams.

### Model Depth

- Core subdomain models are deep, capturing nuanced clinical reasoning and complex
  business rules.
- Supporting subdomain models are moderately detailed, focusing on correctness and
  integration.
- Generic subdomain models are shallow, capturing only essential workflow steps.

## Subdomain Evolution

Subdomain classification is not permanent. As ophthalmology practice evolves, a
subdomain may shift in strategic importance. For example, if a practice develops a
novel retinal treatment protocol, Retinal Care could transition from supporting to
core. Similarly, advances in AI-assisted diagnostic imaging could elevate Diagnostic
Imaging to core status. The classification should be reviewed annually.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1.
