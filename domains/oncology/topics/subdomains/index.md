# Subdomains in Oncology

## Overview

Subdomains classify domain areas by their strategic importance to the organization. In Domain-Driven Design, subdomains are categorized as core, supporting, or generic. This classification guides investment decisions, determining where to apply the most sophisticated modeling techniques, where to use simpler approaches, and where off-the-shelf solutions suffice. In oncology, subdomain classification reflects which capabilities provide clinical differentiation and which are commoditized infrastructure.

## Core Subdomains

Core subdomains represent the highest strategic value and require the most sophisticated domain modeling. They are the source of competitive advantage and clinical excellence.

### Treatment Decision Support

The process of synthesizing diagnostic data, staging information, molecular profiles, patient preferences, and evidence-based guidelines into individualized treatment recommendations is a core subdomain. This is where the deepest clinical expertise is applied, and where the quality of the domain model most directly affects patient outcomes. Modeling treatment decision logic requires capturing the nuanced reasoning of multidisciplinary tumor boards.

### Chemotherapy Dose Optimization

Calculating optimal chemotherapy doses based on patient-specific parameters (body surface area, organ function, comorbidities, prior toxicities) is a core subdomain. Dosing errors can be life-threatening, and the algorithms that determine dose modifications based on toxicity grades and protocol rules represent critical domain knowledge that must be modeled with precision.

### Radiation Treatment Planning

The optimization of radiation dose distributions to maximize tumor coverage while minimizing exposure to healthy tissue is a core subdomain. This involves complex clinical constraints, dose-volume relationships, and treatment technique selection that require deep domain expertise to model accurately.

## Supporting Subdomains

Supporting subdomains are necessary for the system to function but do not provide competitive differentiation. They require domain knowledge but follow established patterns.

### Pathology Reporting

Pathology reporting follows standardized formats (synoptic reporting templates from the College of American Pathologists) and classification systems (WHO tumor classification). While essential for diagnosis, the reporting structure itself is standardized across institutions. The domain model must accurately capture report elements but does not require novel modeling approaches.

### Cancer Staging

TNM staging follows the AJCC classification system with well-defined rules for combining T, N, and M categories into overall stage groups. The staging logic is complex but codified in published standards. The domain model implements these published rules rather than inventing new classification approaches.

### Survivorship Care Planning

Survivorship care plans follow published guidelines from ASCO and NCCN that specify recommended follow-up schedules and monitoring based on cancer type, stage, and treatments received. The domain model implements these guideline-driven recommendations.

### Clinical Trial Matching

Matching patients to eligible clinical trials based on diagnosis, stage, biomarkers, and prior treatments requires domain knowledge but follows structured eligibility criteria. This subdomain supports the core treatment planning process.

## Generic Subdomains

Generic subdomains can be addressed with standard solutions that do not require oncology-specific modeling.

### Patient Demographics

Basic patient identity management, contact information, and insurance details are generic across healthcare domains. Standard identity management patterns apply without oncology-specific customization.

### Appointment Scheduling

Scheduling infrastructure for clinic visits, infusion appointments, and simulation sessions uses generic scheduling algorithms. While oncology appointments have specific constraints (infusion duration, treatment day requirements), the scheduling engine itself is generic.

### Document Management

Storing and retrieving clinical documents, consent forms, and correspondence is a generic capability addressed by standard document management approaches.

### Notification and Messaging

Sending alerts, reminders, and communications to patients and providers uses standard messaging infrastructure without requiring oncology-specific domain modeling.

## Strategic Investment Implications

The subdomain classification directly informs resource allocation. Core subdomains warrant investment in deep domain modeling, close collaboration with clinical experts, and iterative refinement. These are areas where a rich domain model provides the greatest return.

Supporting subdomains benefit from solid modeling but do not require the same depth. Pattern-based approaches and established frameworks are appropriate. The domain model should be accurate but does not need to capture every clinical nuance.

Generic subdomains should use proven off-the-shelf solutions or well-established patterns. Custom development in generic subdomains diverts resources from areas of strategic value.

## Subdomain Boundaries and Bounded Contexts

Subdomains and bounded contexts are related but distinct concepts. A bounded context is a design decision about where to draw model boundaries. A subdomain is an analytical classification of domain areas. Multiple subdomains may exist within a single bounded context, and a single subdomain may span multiple bounded contexts in rare cases.

In oncology, the Chemotherapy Management Context contains the core dose optimization subdomain along with the supporting regimen catalog subdomain. The Diagnosis and Staging Context contains the supporting pathology reporting and cancer staging subdomains. This nesting of subdomains within bounded contexts allows each context to apply appropriate modeling depth to its constituent subdomains.

## Evolution of Subdomain Classification

Subdomain classifications are not permanent. As precision medicine advances, molecular profiling may shift from a supporting subdomain within Diagnosis and Staging to a core subdomain driving treatment selection. As artificial intelligence matures, treatment decision support may become increasingly automated, potentially shifting from core to supporting. Regular reassessment ensures that investment remains aligned with strategic priorities.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 15: Distillation.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 2: Domains, Subdomains, and Bounded Contexts.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 1: Analyzing Business Domains.
