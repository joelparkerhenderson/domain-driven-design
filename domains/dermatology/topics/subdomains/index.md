# Subdomains in the Dermatology Domain

Subdomains classify areas of the dermatology domain by their strategic importance to the organization. Following the framework established by Evans (2003) and refined by Khononov (2021), each subdomain is categorized as core, supporting, or generic based on its competitive differentiation value, complexity, and availability of existing solutions. This classification guides investment decisions, team allocation, and technical strategy.

## Core Subdomains

Core subdomains represent the highest-value differentiating capabilities of the dermatology system. These areas embody specialized clinical expertise, require deep domain modeling, and provide the primary competitive advantage. Core subdomains justify the greatest investment in domain modeling, expert collaboration, and iterative refinement.

### Clinical Assessment

Clinical assessment is a core subdomain because accurate skin evaluation is the foundation of all dermatology practice. The ability to systematically examine skin, document findings using dermoscopy, classify lesions accurately, and apply morphological analysis (ABCDE criteria, dermoscopic pattern recognition) is what distinguishes expert dermatologic care. This subdomain requires sophisticated modeling of examination workflows, lesion characterization, anatomical mapping, and classification systems. The domain model must capture the clinical reasoning process that experienced dermatologists apply when evaluating skin conditions.

### Lesion Management

Lesion management is a core subdomain because it coordinates the critical decisions that determine patient outcomes: when to biopsy, what type of biopsy to perform, how to plan excisions with appropriate margins, and how to schedule follow-up based on risk stratification. The complexity of managing lesions through their full lifecycle -- from initial detection through definitive diagnosis and treatment -- requires deep domain modeling. Business rules governing biopsy indications, margin requirements by diagnosis, and follow-up intervals encode essential clinical knowledge.

### Procedure Management

Procedure management is a core subdomain because specialized dermatologic procedures such as Mohs micrographic surgery, cryotherapy, laser therapy, and phototherapy require precise protocol management. Each procedure type has unique parameters, staging requirements, and safety constraints. Mohs surgery, for example, involves multi-stage tissue excision with real-time histologic examination, requiring careful tracking of layers, margins, and defect reconstruction. The procedural expertise encoded in this subdomain is highly specialized and differentiating.

## Supporting Subdomains

Supporting subdomains provide essential functionality that enables core operations but are less uniquely differentiating. These areas are important but follow more standardized patterns and may not require the same depth of custom domain modeling.

### Cosmetic Services

Cosmetic services is a supporting subdomain because while important to many dermatology practices, the workflows for aesthetic consultations, injectable treatments, and skin rejuvenation follow relatively standardized patterns across the industry. The consent process, product tracking, and treatment area documentation are important but not as deeply complex as clinical assessment or surgical management. Cosmetic services may leverage more standardized solution patterns while still requiring domain-specific customization for treatment planning and outcome documentation.

### Treatment Outcomes

Treatment outcomes is a supporting subdomain because outcome measurement, while increasingly important for evidence-based practice, relies on well-established scoring systems (PASI, SCORAD, DLQI) and standardized assessment methodologies. The value lies in consistent application and longitudinal tracking rather than in novel domain modeling. This subdomain supports core operations by providing the feedback data needed to evaluate and refine clinical and procedural approaches.

## Generic Subdomains

Generic subdomains solve common problems with well-established solutions. These areas should leverage existing standards and technologies rather than building custom solutions.

### Pathology Integration

Pathology integration is a generic subdomain because laboratory information systems, specimen tracking, and histopathology reporting follow well-defined interoperability standards including HL7, FHIR, and standard pathology reporting formats. Most dermatology practices integrate with external pathology laboratories that have their own established systems. The dermatology domain should focus on translating between the laboratory system's language and the domain's internal model through anti-corruption layers, rather than rebuilding pathology laboratory capabilities.

## Strategic Implications

The subdomain classification directly influences technical and organizational decisions. Core subdomains warrant the most experienced developers working closely with domain experts, using the richest tactical patterns (aggregates, domain events, specifications). Supporting subdomains may use simpler patterns and can be developed by less specialized teams. Generic subdomains should maximize reuse of existing solutions and standards.

Vernon (2013) emphasizes that misclassifying subdomains leads to either over-engineering generic capabilities or under-investing in core differentiators. Regular reassessment is necessary as the dermatology domain evolves. For example, as personalized medicine advances, treatment outcomes may shift from a supporting to a core subdomain if predictive outcome modeling becomes a key differentiator.

## Investment Priorities

Core subdomains receive the highest investment in domain modeling, expert collaboration, and iterative refinement. Supporting subdomains receive moderate investment focused on reliability and usability. Generic subdomains receive minimal custom development investment, relying instead on integration with existing systems and standards.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 15 on distillation and core domain identification.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 2 on subdomains and bounded contexts.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 1 on subdomain classification and strategic analysis.
