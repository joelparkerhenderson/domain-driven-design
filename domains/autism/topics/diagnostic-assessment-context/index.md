# Diagnostic Assessment Context: Autism Domain

The Diagnostic Assessment Context encompasses all activities related to screening, evaluating, and formally diagnosing autism spectrum disorder, serving as the foundational upstream bounded context in the autism domain.

## Purpose

Accurate and timely diagnostic assessment is the entry point for all subsequent autism services. This bounded context manages the complete diagnostic pathway from initial screening through formal diagnosis and report generation. It ensures that standardized instruments are administered correctly, DSM-5 criteria are systematically applied, and multi-disciplinary team assessments produce reliable diagnostic determinations that inform all downstream intervention and educational planning.

## Domain Model

### Aggregates

The Diagnostic Evaluation Aggregate is the primary aggregate root, containing the complete evaluation process for one individual. It enforces the invariant that a formal diagnosis requires completion of at least one standardized diagnostic instrument plus documented clinical observation. The evaluation progresses through defined lifecycle states: Initiated, Screening Complete, Instruments Administered, Determination Made, and Report Finalized.

The Assessment Instrument Aggregate manages individual instrument administrations (ADOS-2, ADI-R, Bayley-4, Vineland-3). It enforces scoring validity rules specific to each instrument, ensuring that required items are completed and scores fall within valid ranges.

### Key Entities

The Individual Under Evaluation carries a persistent identity throughout the diagnostic process. The Assessment Session entity tracks each scheduled evaluation appointment. The Clinician entity represents diagnostic professionals with their credentials, licensure, and scope of practice.

### Key Value Objects

DSM5DiagnosticCode captures the specific diagnostic classification. ADOS2Score represents scored results from the ADOS-2 instrument including module, domain scores, comparison scores, and classification. SeverityLevel captures the DSM-5 three-tier support needs classification. DevelopmentalAge represents developmental levels across cognitive, language, motor, adaptive, and social-emotional domains.

## Screening Process

The screening process identifies individuals who may benefit from a comprehensive diagnostic evaluation. The M-CHAT-R/F (Modified Checklist for Autism in Toddlers, Revised with Follow-Up) is the primary screening tool for toddlers aged 16-30 months. The screening process proceeds in two stages: the initial 20-item parent questionnaire, followed by structured follow-up interview items for positive screens. Scoring classifies risk as low (score 0-2), medium (score 3-7), or high (score 8-20).

For older individuals, broader developmental screening tools and autism-specific questionnaires (Social Communication Questionnaire, Social Responsiveness Scale) are administered. Screening results alone never constitute a diagnosis -- they indicate whether further evaluation is warranted.

## Diagnostic Instruments

### ADOS-2 (Autism Diagnostic Observation Schedule, Second Edition)

The ADOS-2 is a semi-structured, standardized observation assessment. The clinician selects one of five modules based on the individual's age and language level. The assessment produces domain scores for Social Affect and Restricted and Repetitive Behavior, which are combined into an Overall Total. Comparison scores (1-10) contextualize the total score relative to individuals with the same age and language level. The algorithm classification (non-spectrum, autism spectrum, autism) provides a standardized categorization.

### ADI-R (Autism Diagnostic Interview, Revised)

The ADI-R is a structured caregiver interview assessing early development, language and communication, reciprocal social interaction, and restricted/repetitive behaviors. Scores are evaluated against diagnostic algorithm cutoffs. The ADI-R is typically used in combination with the ADOS-2 for a comprehensive assessment.

### Developmental Assessments

Cognitive assessments (Bayley-4, WISC-V, Stanford-Binet 5), adaptive behavior assessments (Vineland-3, ABAS-3), and developmental assessments provide context for the autism-specific diagnostic instruments and inform support level determination.

## DSM-5 Diagnostic Criteria

The DSM-5 defines autism spectrum disorder as meeting criteria in two domains:

Criterion A: Persistent deficits in social communication and social interaction across multiple contexts, manifested in deficits in social-emotional reciprocity, nonverbal communicative behaviors, and developing/maintaining/understanding relationships.

Criterion B: Restricted, repetitive patterns of behavior, interests, or activities, manifested in at least two of four specified areas: stereotyped/repetitive motor movements, insistence on sameness, restricted/fixated interests, and hyper-/hyporeactivity to sensory input.

Additional criteria require that symptoms be present in the early developmental period, cause clinically significant impairment, and are not better explained by intellectual disability or global developmental delay.

## Multi-Disciplinary Team Assessment

Complex diagnostic cases require evaluation by a multi-disciplinary team, which may include a developmental pediatrician, clinical psychologist, speech-language pathologist, occupational therapist, and social worker. The team integrates findings from multiple assessment domains to produce a consensus diagnostic determination. The Diagnostic Evaluation Aggregate coordinates team member contributions while enforcing that all required assessment domains are addressed before a determination is finalized.

## Domain Events

Key events produced by this context include ScreeningCompleted, DiagnosticEvaluationInitiated, DiagnosisConfirmed, and DiagnosticReportFinalized. The DiagnosisConfirmed event is the most significant, triggering downstream actions in all other bounded contexts.

## Integration Points

This context publishes diagnostic information to all downstream contexts through published language interfaces. Anti-corruption layers translate clinical diagnostic terminology into educational eligibility language for the Educational Planning Context and into intervention-relevant formats for the Behavioral Intervention, Sensory Processing, and Communication Support contexts.

## References

- American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders* (5th ed.). APA Publishing.
- Lord, C., et al. (2012). *ADOS-2 Manual*. Western Psychological Services.
- Rutter, M., Le Couteur, A., & Lord, C. (2003). *ADI-R Manual*. Western Psychological Services.
- Robins, D. L., Fein, D., & Barton, M. (2009). Modified Checklist for Autism in Toddlers, Revised, with Follow-Up (M-CHAT-R/F). Pediatrics, 133(1), 37-45.
- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
