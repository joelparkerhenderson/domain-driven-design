# Cosmetic Services Context

The Cosmetic Services Context is a supporting bounded context in the dermatology domain that manages elective aesthetic services. It owns aesthetic consultation records, injectable treatment plans (neuromodulators and dermal fillers), chemical peel protocols, microneedling sessions, skin rejuvenation programs, consent workflows, treatment area mapping, and product lot tracking. This context operates under different regulatory, consent, and billing frameworks than medical dermatology contexts, reflecting the distinct nature of cosmetic practice.

## Context Purpose

The Cosmetic Services Context exists to model the business rules and workflows unique to aesthetic dermatology. Cosmetic procedures are elective, require specific informed consent processes, involve product tracking with lot numbers and expiration dates, and have distinct outcome measurement criteria based on patient aesthetic satisfaction rather than disease resolution. Separating cosmetic services into its own bounded context prevents the mixing of medical and cosmetic business rules, which is important for both regulatory compliance and clinical clarity.

## Aggregates

### Cosmetic Treatment Plan Aggregate

The primary aggregate is the Cosmetic Treatment Plan. The root entity carries the plan identifier, patient reference, consulting provider, aesthetic goals documented during consultation, and planned treatment timeline. Within this aggregate, InjectableTreatment entities track individual injection sessions with product type, product name, lot number, units or volume administered per treatment area, injection technique, and immediate post-treatment assessment. ChemicalPeel entities capture peel type (superficial, medium, deep), acid composition and concentration, application duration, and neutralization protocol. MicroneedlingSession entities record needle depth, treatment areas, and any topical agents applied.

The aggregate enforces several critical invariants. A valid Cosmetic Consent must exist and be non-expired before any treatment can be administered. Product lot numbers must be validated against expiration dates, rejecting treatments with expired products. Maximum dosage limits per treatment area must not be exceeded for injectable products. Mandatory waiting periods between certain treatment types must be observed.

### Cosmetic Consent Aggregate

The Cosmetic Consent aggregate manages the informed consent lifecycle for aesthetic procedures. The root entity carries the consent identifier, patient reference, procedure type or types covered, risks and alternatives discussed, product information provided, signature date, and expiration date. The consent entity enforces that all required disclosure elements are documented before the consent can be considered valid. Consent may expire based on elapsed time since signature or become invalid if the planned procedure changes materially.

## Key Entities

### Cosmetic Consultation Entity

The Cosmetic Consultation entity represents the initial aesthetic encounter. It carries the patient's stated aesthetic concerns, provider assessment of facial anatomy and skin quality, treatment recommendations with rationale, expected outcomes discussion, and before-treatment documentation baseline. The entity progresses through states: scheduled, conducted, plan-proposed, and plan-accepted or plan-declined.

### Injectable Treatment Entity

The Injectable Treatment entity records a single injection session. For neuromodulators (such as botulinum toxin), it captures the product name, lot number, total units, units per injection site, injection sites mapped to anatomical landmarks, and dilution ratio. For dermal fillers (such as hyaluronic acid products), it captures the product name, lot number, syringe volume used, injection depth (intradermal, subcutaneous, supraperiosteal), and injection technique (linear threading, fanning, cross-hatching, serial puncture). The entity tracks the immediate post-treatment assessment and any adverse reactions.

### Chemical Peel Entity

The Chemical Peel entity records a single peel treatment session. It captures the peel depth classification, acid type and concentration (glycolic acid, salicylic acid, trichloroacetic acid, phenol), application method, application duration, neutralization timing, and skin response during treatment. The entity also records any post-peel care instructions provided.

## Key Value Objects

The Injectable Dose value object carries the product type, product name, dose quantity (units for neuromodulators, milliliters for fillers), and lot number. The Treatment Area Specification value object defines the anatomical zone targeted for treatment using standardized facial landmarks (forehead, glabella, crow's feet, nasolabial folds, marionette lines, lips, jawline, chin). The Product Lot Information value object carries the manufacturer, product name, lot number, manufacturing date, and expiration date.

## Domain Events Published

CosmeticConsentObtained is published when a patient signs informed consent, enabling treatment to proceed. InjectableTreatmentAdministered is published when an injection session is completed, carrying product and dosage details. CosmeticConsultationCompleted is published when an aesthetic consultation concludes with a proposed treatment plan.

## Domain Events Consumed

This context consumes data from the Clinical Assessment Context's patient skin profile, conforming to its representation of Fitzpatrick skin type and documented skin conditions. This information informs product selection and treatment parameter decisions. For example, certain chemical peel depths are contraindicated for higher Fitzpatrick skin types due to increased risk of post-inflammatory hyperpigmentation.

## Domain Services

The InjectableDosageCalculationService recommends dosages based on treatment area, patient anatomy, and previous treatment history. The CosmeticConsentValidationService verifies that all consent requirements are met before treatment proceeds. These services encapsulate business rules that span multiple value objects and entities.

## Product Tracking

The Cosmetic Services Context maintains rigorous product tracking for patient safety and regulatory compliance. Every injectable product used is documented with its lot number, expiration date, and the specific patient and treatment area where it was administered. This enables rapid identification of affected patients in the event of a product recall and satisfies FDA adverse event reporting requirements.

## Billing and Financial Considerations

Cosmetic services typically operate outside insurance coverage, requiring different billing workflows than medical dermatology. The Cosmetic Services Context models treatment pricing, package deals, and payment tracking as domain concepts specific to elective aesthetic practice. This financial separation reinforces the bounded context boundary between cosmetic and medical services.

## Relationships to Other Contexts

The Clinical Assessment Context is upstream, providing patient skin profile data in a conformist relationship. The Treatment Outcomes Context may consume cosmetic treatment data for aesthetic outcome tracking. The Cosmetic Services Context does not interact directly with the Lesion Management, Procedure Management, or Pathology Integration Contexts, reflecting the natural separation between medical and cosmetic dermatology workflows.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Carruthers, Jean, and Carruthers, Alastair. "Botulinum Toxin in Clinical Dermatology." Springer, 2nd Edition, 2019.
- Kontis, Theda C., and Rivkin, Alexander. "Injectable Fillers in Aesthetic Medicine." Springer, 2019.
