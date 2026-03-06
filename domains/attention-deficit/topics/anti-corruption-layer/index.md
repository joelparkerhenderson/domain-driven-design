# Anti-Corruption Layer: Attention-Deficit Domain

The anti-corruption layer (ACL) is a translation boundary that protects a bounded context's internal domain model from being corrupted by external system concepts, terminology, or data structures. In the ADHD management domain, ACLs are essential at the boundaries between clinical, educational, and administrative systems that use fundamentally different conceptual models.

## Purpose

ADHD management spans clinical healthcare, educational institutions, and administrative systems that were not designed to work together. Without anti-corruption layers, the domain model of one context risks being distorted by the concepts and constraints of another. For example, clinical diagnostic terminology should not leak into educational accommodation planning, and school administrative data structures should not dictate how clinical assessments are modeled.

## Key Anti-Corruption Layers

### Clinical-to-Educational ACL

This is the most critical anti-corruption layer in the ADHD management domain. Clinical assessment produces diagnostic data using medical terminology (DSM-5 codes, symptom severity ratings, neuropsychological test scores, comorbidity profiles). Educational accommodation requires educational terminology (eligibility categories, functional limitations, accommodation needs, instructional modifications).

The ACL translates between these conceptual models:

- **DSM-5 ADHD Diagnosis** translates to **Educational Eligibility Category** (e.g., "Other Health Impairment" under IDEA).
- **Symptom Severity Rating** translates to **Functional Limitation Description** relevant to the classroom setting.
- **Neuropsychological Test Results** translate to **Learning Profile Characteristics** that inform accommodation selection.
- **Clinical Recommendations** translate to **Accommodation Specifications** expressed in educational terms.

### Clinical-to-Medication ACL

When clinical assessment data flows to the Medication Management Context, an ACL ensures that the medication context's pharmacological model is not corrupted by assessment-specific concepts:

- **Diagnostic Impression** translates to **Treatment Indication** in pharmacological terms.
- **Symptom Profile** translates to **Target Symptom Set** for medication selection.
- **Comorbidity Findings** translate to **Contraindication Screening Criteria**.
- **Assessment Severity Rating** translates to **Clinical Severity Threshold** for treatment initiation.

### External System ACLs

ADHD management systems frequently integrate with external systems that have their own data models:

- **Electronic Health Record (EHR) ACL**: Translates between the ADHD domain model and generic EHR data structures, preventing the domain model from conforming to EHR schema limitations.
- **School Information System (SIS) ACL**: Translates between the educational accommodation model and school administrative system data formats.
- **Insurance/Billing ACL**: Translates clinical assessment and treatment data into billing codes and authorization formats without contaminating the clinical model with billing concerns.
- **Laboratory/Testing ACL**: Translates neuropsychological test platform output into the clinical assessment model's internal representation.

## Design Principles

**Translate, do not adopt.** The ACL translates external concepts into the receiving context's language rather than adopting external terminology. The Educational Accommodation Context should never use DSM-5 codes internally; it should use its own eligibility and accommodation vocabulary.

**Isolate change impact.** When external systems change their data formats or conceptual models, only the ACL needs modification. The protected bounded context's internal model remains stable.

**Make translation explicit.** Every translation rule in the ACL should be documented and testable. Hidden or implicit translations create maintenance risks and data integrity issues.

**Handle translation failures gracefully.** Not all external concepts map cleanly to the receiving context's model. The ACL must define how to handle unmappable data, ambiguous translations, and version mismatches.

## ACL Implementation Considerations

Anti-corruption layers in the ADHD management domain are implemented as domain-level translation services, not infrastructure concerns. The translation logic encodes domain knowledge about the relationship between clinical, educational, and pharmacological concepts. This logic belongs in the domain layer, not in integration middleware.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Individuals with Disabilities Education Act (IDEA), 20 U.S.C. 1400 et seq.
