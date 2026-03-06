# Consent to Treatment Domain

## Introduction

This domain applies Domain-Driven Design (DDD) to consent-to-treatment processes in healthcare. Consent to treatment is the legal and ethical requirement that a healthcare provider must obtain a patient's voluntary, informed agreement before performing any examination, investigation, or treatment. It rests on the principle of patient autonomy and requires that the patient receives adequate information about the nature, purpose, risks, benefits, and alternatives of a proposed intervention, and that they have the capacity to make the decision freely.

## Bounded Contexts

1. **Informed Consent** - Manages the end-to-end process of disclosing material information to a patient about a proposed treatment, ensuring the patient understands the risks, benefits, and alternatives, and documenting their voluntary agreement or refusal. This context enforces that disclosure meets legal and professional standards of materiality.

2. **Capacity Assessment** - Handles the clinical evaluation of whether a patient can understand, retain, and weigh relevant information and communicate a decision at the time consent is sought. This context incorporates structured assessment protocols and records the assessor's findings, including any fluctuating or condition-specific capacity considerations.

3. **Proxy Decision-Making** - Governs situations where a patient lacks capacity and a legally authorized surrogate, such as a healthcare proxy, lasting power of attorney holder, or court-appointed deputy, must make treatment decisions. This context manages the validation of proxy authority, the application of best-interests principles, and the documentation of surrogate decisions.

4. **Consent Lifecycle** - Tracks the full lifecycle of a consent record from creation through verification, renewal, modification, withdrawal, and archival. This context ensures that every consent event is timestamped, attributed to a specific actor, and linked to the relevant treatment episode for audit and compliance purposes.

5. **Emergency Authorization** - Addresses situations where treatment must proceed without explicit consent due to clinical urgency and the patient's inability to consent. This context documents the clinical justification, the treatments provided, and the subsequent steps taken to inform the patient or their proxy once practicable.

## Strategic Design

- **Subdomains** classify areas by strategic importance, with informed consent as the core subdomain and capacity assessment, proxy authorization, and lifecycle management as supporting subdomains.
- **Context Map** defines relationships between bounded contexts, such as the upstream dependency of Consent Lifecycle on Informed Consent and Capacity Assessment.
- **Anti-Corruption Layers** protect bounded contexts from external clinical and legal systems.

## Tactical Design

- **Aggregates** enforce consistency boundaries, such as the Consent Record aggregate that groups disclosure, authorization, and signatures.
- **Entities** represent domain objects with unique identity, such as Patient, Clinician, and Consent Form.
- **Value Objects** capture immutable measurements and types, such as Disclosure Content, Risk Description, and Capacity Status.
- **Domain Events** signal significant occurrences, such as ConsentGranted, ConsentWithdrawn, and CapacityDetermined.
- **Repositories** abstract persistence of consent records, capacity assessments, and proxy authorizations.
- **Domain Services** encapsulate cross-aggregate operations, such as validating that all legal prerequisites for consent are met before authorizing treatment.

## References

- Evans, Eric. *Domain-Driven Design*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Beauchamp, Tom L., and James F. Childress. *Principles of Biomedical Ethics*. 8th ed. Oxford University Press, 2019.
- General Medical Council. *Decision Making and Consent*. GMC, 2020.
- Mental Capacity Act 2005, Code of Practice. UK Department for Constitutional Affairs, TSO, 2007.
