# Attention-Deficit Standards: Attention-Deficit Domain

Domain-specific standards inform the design of value objects, published languages, and domain rules throughout the ADHD management domain. These standards represent codified clinical knowledge, regulatory frameworks, and professional guidelines that constrain and shape how the domain model represents ADHD assessment, treatment, and outcomes.

## Purpose

ADHD management is governed by a rich ecosystem of clinical, educational, and regulatory standards. These standards are not optional guidelines but fundamental constraints that shape the domain model. A diagnostic code must conform to DSM-5 or ICD-11 classifications. A rating scale score must use the validated scoring algorithm for the specific instrument. An educational accommodation plan must meet IDEA or Section 504 structural requirements. Standards inform value object design by defining valid ranges, classification systems, and coding schemes that the domain model must enforce.

## Diagnostic Classification Standards

### DSM-5 (Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition)

Published by the American Psychiatric Association (2013), the DSM-5 provides the primary diagnostic criteria for ADHD in the United States. Key elements that inform domain model design:

- **Diagnostic Criteria Structure**: 18 symptoms divided into two dimensions (9 inattention, 9 hyperactivity-impulsivity). Six or more symptoms required for children under 17; five or more for adults aged 17 and older. Symptoms must be present for at least six months and be inconsistent with developmental level.
- **Presentation Type Classification**: Three types (predominantly inattentive, predominantly hyperactive-impulsive, combined) based on which symptom dimensions meet threshold. This classification informs the ADHD Presentation Type value object.
- **Severity Specifiers**: Mild, moderate, or severe based on the number of symptoms exceeding the diagnostic threshold and the degree of functional impairment. This informs the Symptom Severity Level value object.
- **Age of Onset**: Symptoms must have been present before age 12. This constraint informs the developmental history component of the Patient Assessment aggregate.
- **Cross-Setting Impairment**: Symptoms must be present in two or more settings. This constraint informs the multi-informant assessment requirement and cross-setting behavioral monitoring.
- **Exclusionary Criteria**: Symptoms must not be better explained by another mental disorder. This informs the differential diagnosis component of the diagnostic evaluation.

### ICD-11 (International Classification of Diseases, 11th Revision)

Published by the World Health Organization (2019), ICD-11 provides the international diagnostic coding system. ADHD is classified as "Attention Deficit Hyperactivity Disorder" (6A05) with subtypes:

- 6A05.0: Predominantly inattentive presentation.
- 6A05.1: Predominantly hyperactive-impulsive presentation.
- 6A05.2: Combined presentation.
- 6A05.Y: Other specified presentation.
- 6A05.Z: Unspecified presentation.

ICD-11 codes are used alongside DSM-5 codes in the Diagnostic Code value object for international interoperability.

## Clinical Practice Guidelines

### AAP Clinical Practice Guideline (2019)

The American Academy of Pediatrics guideline for diagnosing and treating ADHD in children and adolescents (ages 4-18) informs treatment management domain rules:

- **Age-Based Treatment Recommendations**: For preschool children (ages 4-5), behavioral therapy is recommended as the first-line treatment before medication. For school-age children (ages 6-11), FDA-approved ADHD medication and/or behavioral therapy are recommended. For adolescents (ages 12-18), FDA-approved medication is recommended with behavioral therapy as a desirable adjunct.
- **Titration Requirements**: Medication should be titrated to achieve maximum benefit with minimum adverse effects.
- **Monitoring Frequency**: Follow-up assessments at regular intervals to monitor treatment response and side effects.

### NICE Guideline NG87 (2018)

The National Institute for Health and Care Excellence guideline on ADHD diagnosis and management informs treatment protocols particularly for international contexts:

- **Watchful Waiting**: For children with moderate impairment, environmental modifications should be tried before medication.
- **Group-Based ADHD-Focused Support**: Recommended as first-line treatment for children and young people with moderate impairment.
- **Medication Recommendations**: Methylphenidate as first-line medication for children and young people; lisdexamfetamine as first-line for adults.

## Assessment Instrument Standards

### Conners Rating Scales

- **Conners-3**: Normative data based on large stratified samples. T-scores with a mean of 50 and standard deviation of 10. Clinical significance threshold at T-score of 65 (approximately 93rd percentile). These parameters define the valid range and interpretation rules for the Rating Scale Score value object.
- **Conners-4**: Updated version with revised normative data and additional content scales. Score interpretation rules inform the RatingScaleInterpretationService.

### SNAP-IV

- **Scoring**: Each item rated 0-3 (Not at All, Just a Little, Quite a Bit, Very Much). Average scores calculated for inattention subscale (items 1-9) and hyperactivity-impulsivity subscale (items 10-18). Clinical cutoff at the 95th percentile of normative data (approximately 1.8 for inattention, 1.44 for hyperactivity-impulsivity).

### Vanderbilt Assessment Scales

- **Parent and Teacher Forms**: Symptom items mapped directly to DSM criteria. Performance items assessing functional impairment in academic and social domains. Scoring combines symptom count with performance impairment to support diagnostic determination.

## Educational Standards

### IDEA (Individuals with Disabilities Education Act)

Defines the framework for IEP development and special education services. Key standards informing the Educational Accommodation Context:

- **Eligibility Categories**: ADHD typically qualifies under "Other Health Impairment" (OHI).
- **IEP Requirements**: Present levels of performance, measurable annual goals, special education services, supplementary aids and services, participation in general education, assessment accommodations, and transition planning (age 16+).
- **Procedural Safeguards**: Parent notification, consent requirements, due process rights, and evaluation timelines.

### Section 504 of the Rehabilitation Act

Defines the framework for 504 accommodation plans. Key standards:

- **Eligibility**: Physical or mental impairment that substantially limits one or more major life activities.
- **Major Life Activities**: Learning, reading, concentrating, thinking, communicating.
- **Plan Requirements**: Documentation of disability, identification of impacted activities, specification of accommodations.

## Pharmacological Standards

- **FDA Approved Indications**: Approved age ranges and dosage guidelines for each ADHD medication.
- **DEA Scheduling**: Schedule II classification for stimulant medications, governing prescribing, dispensing, and refill rules.
- **Black Box Warnings**: Required monitoring for specific medications (e.g., cardiovascular risk with stimulants, suicidal ideation risk with atomoxetine in pediatric patients).

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- American Psychiatric Association. (2013). *Diagnostic and Statistical Manual of Mental Disorders* (5th ed.).
- World Health Organization. (2019). *International Classification of Diseases* (11th ed.).
- Wolraich, M. L., et al. (2019). Clinical Practice Guideline for the Diagnosis, Evaluation, and Treatment of ADHD. *Pediatrics*, 144(4).
- National Institute for Health and Care Excellence. (2018). *ADHD: Diagnosis and Management* (NG87).
