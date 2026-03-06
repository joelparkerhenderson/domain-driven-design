# Subdomains in the Gerontology Domain

## Overview

Subdomain classification is a strategic design activity that categorizes areas of a domain by their strategic importance to the organization. Evans (2003) and Khononov (2021) identify three types of subdomains: core, supporting, and generic. This classification guides resource allocation, development priorities, and build-versus-buy decisions. In the gerontology domain, subdomain classification reflects which aspects of geriatric care provide the most clinical and organizational value.

## Core Subdomains

Core subdomains represent the areas where the organization differentiates itself and creates the most value. These subdomains require the deepest domain expertise, the most careful modeling, and the highest development investment.

### Geriatric Assessment

Comprehensive geriatric assessment is a core subdomain because it is the foundational clinical process that distinguishes geriatric care from general medicine. The CGA integrates medical, functional, psychological, and social evaluations into a unified assessment that guides all subsequent care decisions. The complexity of multidimensional assessment, the need for clinical decision support, and the integration of multiple screening instruments make this a high-value area requiring custom modeling.

### Care Coordination

Care coordination is a core subdomain because effective coordination across interdisciplinary teams and care settings is what determines outcomes for older adults. The fragmented nature of healthcare delivery for aging populations makes coordination both challenging and valuable. Modeling care transitions, team communication, caregiver support, and community resource linkage requires deep domain knowledge and careful aggregate design.

## Supporting Subdomains

Supporting subdomains are necessary for the domain to function but do not provide primary differentiation. They require domain-specific knowledge but can be built with less complexity than core subdomains.

### Cognitive Health

Cognitive health screening and management is a supporting subdomain. While critical to geriatric care, cognitive testing instruments (MMSE, MoCA) are well-standardized, and the workflows for dementia staging and memory care planning follow established clinical protocols. The subdomain supports the core assessment and coordination functions by providing specialized cognitive data.

### Functional Independence

Functional independence assessment is a supporting subdomain. ADL and IADL assessments use established instruments (Katz Index, Lawton Scale), and the workflows for mobility aid prescription and home modification are relatively standardized. This subdomain supports care coordination by providing functional status data that informs care plan decisions.

### Medication Management

Medication management is a supporting subdomain. While polypharmacy is a significant clinical concern in gerontology, medication review processes, Beers criteria application, and deprescribing protocols follow established clinical guidelines. This subdomain supports the core subdomains by ensuring medication safety and appropriateness.

### End of Life Planning

End-of-life planning is a supporting subdomain. Advance directive documentation, palliative care referral, and hospice enrollment follow legal and clinical frameworks that are well-defined. The subdomain supports care coordination by ensuring that treatment decisions align with patient values and legal requirements.

## Generic Subdomains

Generic subdomains provide functionality required by the domain but not specific to gerontology. These areas can typically leverage commercial off-the-shelf solutions, open-source libraries, or shared platform services.

### Scheduling and Appointments

Appointment scheduling for geriatric assessments, care team meetings, and follow-up visits is a generic subdomain. Standard scheduling solutions can be adapted to the gerontology domain without custom modeling.

### Notifications and Alerts

Sending reminders for medication adherence, follow-up assessments, and care plan reviews is a generic subdomain. Standard notification infrastructure can handle these requirements.

### User Authentication and Authorization

Managing access control for clinicians, caregivers, and patients is a generic subdomain governed by standard security practices and HIPAA requirements.

### Document Management

Storing and retrieving clinical documents, advance directives, and assessment reports is a generic subdomain that can leverage standard document management solutions.

## Resource Allocation Implications

Core subdomains receive the best developers, the most modeling time, and the highest architectural attention. Supporting subdomains receive adequate resources but may use simpler architectural patterns. Generic subdomains are candidates for buy-or-borrow decisions, minimizing custom development.

Khononov (2021) emphasizes that misclassifying subdomains leads to either overinvestment in generic functionality or underinvestment in core differentiators. Regular reassessment of subdomain classification ensures that resources align with strategic priorities as the domain evolves.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
