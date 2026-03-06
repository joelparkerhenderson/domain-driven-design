# Ubiquitous Language: Attention-Deficit Domain

The ubiquitous language is the shared vocabulary used consistently by domain experts, developers, and stakeholders across the ADHD management domain. It bridges the gap between clinical, educational, and therapeutic terminology, ensuring that all participants in the system share a precise, unambiguous understanding of domain concepts.

## Purpose

ADHD management involves professionals from diverse disciplines: psychiatrists, psychologists, pediatricians, educators, behavioral therapists, coaches, and pharmacists. Each discipline brings its own terminology. The ubiquitous language unifies these perspectives within each bounded context, ensuring that a term like "assessment" or "accommodation" carries a precise, agreed-upon meaning wherever it appears in the domain model, codebase, and documentation.

## Language Development Process

The ubiquitous language for the attention-deficit domain is developed collaboratively:

1. **Clinical terminology review**: Clinicians and psychologists contribute terms from DSM-5, rating scale manuals, and clinical practice guidelines.
2. **Educational terminology alignment**: Educators contribute terms from IDEA, Section 504, and IEP/504 plan frameworks.
3. **Therapeutic terminology integration**: Behavioral therapists and coaches contribute terms from evidence-based intervention protocols.
4. **Pharmacological terminology inclusion**: Prescribers contribute terms from medication management guidelines and pharmacological references.
5. **Cross-disciplinary harmonization**: The team resolves conflicts where different disciplines use the same term with different meanings or different terms for the same concept.

## Key Language Boundaries

Different bounded contexts may use different terms for overlapping concepts, and this is by design:

- The **Clinical Assessment Context** uses "patient" and "diagnostic evaluation."
- The **Educational Accommodation Context** uses "student" and "eligibility determination."
- The **Treatment Management Context** uses "client" and "treatment episode."
- The **Medication Management Context** uses "patient" and "medication trial."

These are not inconsistencies but rather reflections of each context's distinct perspective. Translation occurs at context boundaries through anti-corruption layers and published languages.

## Core Domain Terms

The following terms form the foundation of the ubiquitous language across the ADHD management domain. Each term has a single, precise definition agreed upon by all stakeholders within the relevant bounded context.

- **ADHD Presentation Type**: Combined, predominantly inattentive, or predominantly hyperactive-impulsive classification.
- **Assessment Session**: A discrete clinical encounter during which standardized evaluation instruments are administered.
- **Behavior Chart**: A structured recording instrument that tracks the frequency, duration, and intensity of specific target behaviors over time.
- **Comorbidity Profile**: The set of co-occurring conditions alongside ADHD that influence assessment interpretation and treatment planning.
- **Diagnostic Impression**: The clinician's formalized conclusion regarding ADHD diagnosis, presentation type, severity, and comorbidities based on comprehensive assessment.
- **Executive Function Deficit**: A measurable impairment in cognitive control processes such as working memory, cognitive flexibility, planning, or inhibitory control.
- **Functional Impairment Rating**: A quantified measure of how ADHD symptoms impact daily functioning across academic, social, occupational, and family domains.
- **Multimodal Treatment Plan**: An integrated intervention strategy combining two or more treatment modalities tailored to the individual's symptom profile and functional needs.
- **Rating Scale Administration**: The standardized process of administering, scoring, and interpreting a validated ADHD symptom questionnaire.
- **Symptom Severity Level**: A categorized measure (mild, moderate, severe) of ADHD symptom intensity based on standardized assessment criteria.
- **Treatment Response**: The documented change in symptom severity and functional impairment following a specific treatment intervention.
- **Titration Schedule**: A planned sequence of medication dosage adjustments designed to identify the optimal therapeutic dose.

## Language Governance

The ubiquitous language is a living artifact that evolves as the domain understanding deepens. Changes to the language follow a governance process:

- New terms are proposed by domain experts and validated across disciplines.
- Existing terms are revised when clinical practice guidelines are updated (e.g., DSM-5 revisions, new AAP recommendations).
- Deprecated terms are documented with migration guidance.
- The ubiquitous language glossary (ubiquitous-language.md) serves as the authoritative reference.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders* (5th ed.).
