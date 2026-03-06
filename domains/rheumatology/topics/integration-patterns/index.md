# Integration Patterns in the Rheumatology Domain

## Overview

Integration patterns define how bounded contexts interact, share data, and maintain consistency across context boundaries. In the rheumatology domain, integration patterns must balance the need for clinical data sharing (an assessment must reach the disease management context) with the need for context autonomy (each context must maintain its own model and rules). The patterns selected for each integration point reflect the nature of the relationship between the contexts involved and the degree of coupling acceptable.

## Shared Kernel

### Joint Examination Terminology

The Clinical Assessment Context and the Autoimmune Disease Context share a small, carefully governed kernel of common definitions. Both contexts must agree on what constitutes a "tender joint" (pain on palpation or during passive movement), a "swollen joint" (soft tissue enlargement detected during examination), and which joints are included in the 28-joint count (PIPs, MCPs, wrists, elbows, shoulders, knees bilaterally).

This shared kernel is minimal by design. It includes only the definitions that absolutely must be consistent across contexts. Changes to these definitions require agreement from both context teams. The kernel is versioned and treated as a shared library that both contexts depend upon.

### Disease Activity Categories

The Clinical Assessment Context and the Outcomes Tracking Context share definitions of disease activity categories: remission (DAS28 below 2.6), low (2.6-3.2), moderate (3.2-5.1), and high (above 5.1). These categories must be consistent because the assessment context produces them and the outcomes context stores and trends them. Any change to threshold values must be synchronized.

## Published Language

### Assessment Results Language

The Clinical Assessment Context publishes assessment results using a published language based on ACR/EULAR standard definitions. This language defines the structure and semantics of assessment data: DAS28Score includes component values and composite score with interpretation; SerologyResult includes test type, value, units, reference range, and positivity determination; JointCount includes examination scheme and individual joint findings.

All downstream contexts (Autoimmune Disease, Joint Disease, Outcomes Tracking) consume assessment data using this published language. The language acts as a contract that the Clinical Assessment Context commits to maintaining. Changes to the published language follow a versioning strategy that allows consumers time to adapt.

### Therapy Status Language

The Biologic Therapy Context publishes therapy status using a language that describes therapy lifecycle states (planned, screening, active, suspended, discontinued), drug identification (using WHO INN names and ATC codes), and safety events (infusion reactions, adverse events). This language is consumed by the Outcomes Tracking Context and, for therapy status updates, by the Autoimmune Disease Context.

## Open Host Service

### Biologic Therapy Request Service

The Biologic Therapy Context exposes an open host service for therapy requests. Any context that needs to initiate biologic therapy submits a request through this service. The request format includes patient identifier, requesting context, indication, prior treatments failed, contraindications, and preferred drug class.

The service is "open" because multiple upstream contexts can interact with it using the same protocol. Currently, the Autoimmune Disease Context is the primary consumer, but the Joint Disease Context uses the same service for biologic requests in refractory gout (anakinra) or erosive osteoarthritis.

The service returns a standardized response including screening status, approval status, and estimated start date. This decouples the requesting context from the internal prescribing workflow.

### Referral Service

The Pain Management Context exposes an open host service for therapy referrals. Both the Autoimmune Disease Context and the Joint Disease Context can submit referrals through this service, providing diagnosis, functional limitations, and requested therapy type. The service returns referral acknowledgment and subsequent progress updates.

## Customer-Supplier

### Assessment to Disease Management

The relationship between the Clinical Assessment Context (supplier) and the Autoimmune Disease Context (customer) is a customer-supplier pattern. The Autoimmune Disease Context defines what assessment data it needs to perform disease classification and flare detection. The Clinical Assessment Context accommodates these requirements by ensuring its published assessment results include the necessary data elements.

The customer (Autoimmune Disease Context) has input into the supplier's roadmap. If the disease context needs a new type of assessment data (such as a new disease activity index), it negotiates with the assessment context to add this to the published language.

### Therapy to Outcomes

The relationship between the Biologic Therapy Context (supplier) and the Outcomes Tracking Context (customer) follows the same pattern. The outcomes context defines what therapy data it needs for longitudinal tracking (start dates, drug changes, adverse events, biosimilar switches), and the therapy context ensures its published events carry this information.

## Conformist

### External Drug Registries

The Biologic Therapy Context has a conformist relationship with external drug registry systems. It must accept drug identification, formulary status, and regulatory information in the formats defined by external systems (FDA NDC, WHO ATC). The context cannot influence these external formats and must conform to them, using an anti-corruption layer to translate external concepts into its internal model.

### Quality Reporting Systems

The Outcomes Tracking Context conforms to external quality reporting system requirements (CMS MIPS, HEDIS). It must produce reports in the formats and with the measure definitions specified by these external programs. The anti-corruption layer translates internal outcome data into the required submission formats.

## Separate Ways

### Pain Management and Biologic Therapy

The Pain Management Context and the Biologic Therapy Context operate as separate ways. They have no direct integration. Pain management handles non-biologic interventions (NSAIDs, therapy referrals), while biologic therapy handles immunomodulatory agents. When both contexts serve the same patient, they operate independently, with the Outcomes Tracking Context serving as the shared downstream aggregator.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapters 3, 13.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapters 8-9.
- Hohpe, Gregor, and Bobby Woolf. Enterprise Integration Patterns. Addison-Wesley, 2003.
