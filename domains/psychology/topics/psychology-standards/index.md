# Psychology Standards in the Psychology Domain

## Overview

Domain-specific standards inform the design of value objects, validation rules, and published languages within the psychology domain model. Psychology practice is governed by a comprehensive framework of professional standards, diagnostic classification systems, ethical codes, testing standards, and measurement conventions. These standards are not merely external constraints -- they shape the internal structure of the domain model, defining how clinical concepts are represented, validated, and communicated.

In Domain-Driven Design, standards that inform the domain model should be explicitly recognized and incorporated rather than treated as afterthoughts. The psychology domain is rich in standards because the profession has invested decades in developing rigorous frameworks for assessment, treatment, research, and ethical practice.

## Diagnostic Classification Systems

The Diagnostic and Statistical Manual of Mental Disorders (DSM-5-TR) is the primary diagnostic classification system used in psychology practice in the United States. It defines mental health diagnoses with specific diagnostic criteria, severity specifiers, and course specifiers. The domain model's Diagnosis value object must conform to DSM-5-TR structure, including the diagnostic code, the full diagnostic name, and applicable specifiers.

The International Classification of Diseases (ICD-10-CM and ICD-11) provides an alternative classification system used internationally and required for insurance billing in the United States. The domain model must support both DSM and ICD coding, and the mapping between them, because different contexts and stakeholders may require different classification systems for the same clinical presentation.

## Testing Standards

The Standards for Educational and Psychological Testing, jointly published by the American Educational Research Association (AERA), the American Psychological Association (APA), and the National Council on Measurement in Education (NCME), define the technical requirements for psychological tests. These standards govern test development, reliability, validity, fairness, and the responsibilities of test users.

The domain model incorporates these standards through the Assessment Instrument entity and its associated value objects. Reliability Coefficient, Validity Evidence, Standard Error of Measurement, and Normative Data value objects reflect the psychometric properties that the testing standards require. The model enforces that assessment interpretation references these properties and that normative comparisons use appropriate reference groups.

## Ethical Standards

The APA Ethical Principles of Psychologists and Code of Conduct establishes the ethical framework for psychology practice. Key ethical principles that shape the domain model include informed consent, confidentiality, competence, and multiple relationships.

Informed consent requirements shape the Consent Record value object, ensuring that it captures the information provided, the client's understanding, and the voluntary nature of consent. Confidentiality requirements shape how clinical data is stored, accessed, and shared across contexts, including the anti-corruption layers that control information flow. Competence requirements influence the Assessment entity by linking test administration to qualified practitioners.

The Behavior Analyst Certification Board (BACB) Ethics Code similarly governs the Behavioral Analysis Context, establishing standards for behavior assessment, intervention, and supervision that shape the Behavior Plan aggregate.

## Measurement Standards

The psychology domain uses several established measurement frameworks.

Classical Test Theory (CTT) provides the foundational measurement model. A test score is composed of a true score and error. The Standard Error of Measurement quantifies the expected range of error, and the Reliable Change Index uses this to determine whether observed change exceeds measurement error.

Item Response Theory (IRT) provides an alternative measurement framework used in computer-adaptive testing and modern test development. IRT models the probability of a correct response as a function of the examinee's ability and the item's difficulty, discrimination, and guessing parameters.

Clinically Significant Change methodology, developed by Jacobson and Truax, defines how to determine whether treatment produces meaningful change. The domain model's Reliable Change Index and Clinical Significance Threshold value objects directly implement this methodology.

## Research Standards

The APA Publication Manual (7th edition) establishes standards for reporting research findings. These standards inform the Research Study aggregate and the Manuscript entity, defining how statistical results are reported, how effect sizes are calculated and interpreted, and how research methods are documented.

The CONSORT (Consolidated Standards of Reporting Trials) guidelines specify how randomized controlled trials should be reported. The STROBE guidelines apply to observational studies. The domain model can incorporate these reporting standards as validation rules that ensure research outputs meet publication requirements.

## Practice Guidelines

Clinical practice guidelines published by APA and other professional organizations establish evidence-based recommendations for treating specific conditions. These guidelines inform the Treatment Matching Service, which recommends evidence-based treatments based on diagnostic formulation and client characteristics. Guidelines include the APA Clinical Practice Guideline for PTSD, Depression, and Obesity, among others.

## Cultural and Linguistic Standards

Psychology practice standards increasingly emphasize cultural competence and linguistic appropriateness. The domain model must support the selection of culturally appropriate assessment instruments, the use of normative data from relevant cultural groups, and the documentation of linguistic and cultural factors that may affect assessment and treatment validity. The Normative Comparison value object should indicate the cultural and linguistic characteristics of the normative sample.

## Data Standards

Healthcare data standards such as HL7 FHIR (Fast Healthcare Interoperability Resources) define how clinical data is structured and exchanged between systems. CPT (Current Procedural Terminology) codes define billable services. These standards inform the anti-corruption layers that translate between the psychology domain model and external healthcare systems.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- American Educational Research Association, American Psychological Association, & National Council on Measurement in Education. (2014). Standards for Educational and Psychological Testing.
- American Psychological Association. (2017). Ethical Principles of Psychologists and Code of Conduct.
- American Psychological Association. (2020). Publication Manual of the American Psychological Association (7th ed.).
- Jacobson, N. S., & Truax, P. (1991). Clinical significance: A statistical approach. Journal of Consulting and Clinical Psychology, 59(1), 12-19.
