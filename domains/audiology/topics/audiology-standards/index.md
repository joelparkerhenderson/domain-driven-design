# Audiology Standards in the Domain Model

## Overview

Audiology-specific standards from professional organizations and standards bodies directly inform the design of value objects, business rules, and validation logic in the audiology domain model. These standards define the technical parameters for audiometric testing, the clinical guidelines for assessment and treatment, and the professional requirements for practice. Embedding standards into the domain model ensures that the system enforces clinical best practices by design rather than relying on external compliance checks.

## ANSI Standards

### ANSI S3.6 - Specification for Audiometers

ANSI S3.6 defines the technical requirements for audiometric equipment, including frequency and intensity ranges, output tolerances, calibration specifications, and transducer requirements. This standard directly informs the HearingThreshold value object, which validates that measured thresholds fall within the audiometer's specified output range for the transducer in use. The CalibrationVerificationService references ANSI S3.6 tolerances to determine whether equipment calibration is current and acceptable.

Key parameters from ANSI S3.6 encoded in the domain model include: standard audiometric frequencies (125, 250, 500, 750, 1000, 1500, 2000, 3000, 4000, 6000, 8000 Hz), maximum output levels for air and bone conduction by transducer type, and acceptable calibration tolerances at each frequency.

### ANSI S3.1 - Maximum Permissible Ambient Noise Levels

ANSI S3.1 specifies the maximum ambient noise levels permitted in audiometric test environments by frequency and by transducer type (supra-aural, insert, circumaural). This standard informs the TestEnvironment value object, which validates that the testing environment meets noise requirements before test results are accepted into the domain model.

### ANSI S3.39 - Specification of Hearing Aid Characteristics

ANSI S3.39 defines standardized methods for measuring hearing aid electroacoustic characteristics including gain, output, distortion, and battery current. This standard informs the DeviceSpecification value object in the Device Management Context, providing the reference framework for evaluating device performance against manufacturer specifications.

## ASHA Guidelines

### Scope of Practice in Audiology

The American Speech-Language-Hearing Association's Scope of Practice in Audiology defines the activities that audiologists are qualified to perform. This scope maps directly to the bounded context structure of the audiology domain model: diagnostic assessment (Hearing Assessment Context), device fitting (Device Management Context), rehabilitation (Rehabilitation Context), vestibular services (Vestibular Services Context), and pediatric services (Pediatric Audiology Context).

### Guidelines for Manual Pure-Tone Threshold Audiometry

ASHA guidelines for manual audiometry inform the business rules within the Hearing Assessment Context. These guidelines specify the procedure for threshold search (modified Hughson-Westlake method), the criteria for threshold determination (ascending approach with response at 50% of ascending trials), and the requirements for masking (when to mask, how to determine masking levels, and how to verify adequate masking through plateau technique).

### Guidelines for Hearing Aid Fitting

ASHA guidelines for hearing aid fitting inform the Device Management Context's business rules. These guidelines specify the requirement for prescriptive fitting using validated formulae, the requirement for real ear measurement verification, and the standards for patient orientation and follow-up.

### Documentation Guidelines

ASHA documentation guidelines inform the Clinical Documentation Context's report template structures and compliance validation. These guidelines specify required report elements, documentation timelines, and professional standards for clinical record keeping.

## AAA Clinical Practice Guidelines

### American Academy of Audiology Guidelines

The American Academy of Audiology publishes clinical practice guidelines that complement ASHA guidelines. Key AAA guidelines include: Clinical Practice Guidelines for the Diagnosis and Treatment of Benign Paroxysmal Positional Vertigo (informing the Vestibular Services Context), Pediatric Amplification Guidelines (informing the Pediatric Audiology Context and Device Management Context), and Guidelines for the Audiologic Management of Adult Hearing Impairment (informing the Hearing Assessment and Rehabilitation Contexts).

## JCIH Position Statement

The Joint Committee on Infant Hearing (JCIH) Position Statement defines the EHDI guidelines that the Pediatric Audiology Context enforces. The 2019 JCIH Position Statement specifies: all infants should be screened by one month of age, infants who do not pass screening should have comprehensive audiologic evaluation by three months of age, and infants identified with hearing loss should receive appropriate intervention by six months of age. These timelines are encoded as business rules in the ScreeningFollowUpService.

## Prescriptive Fitting Standards

### NAL-NL2

The National Acoustic Laboratories Non-Linear version 2 (NAL-NL2) prescriptive formula provides target gain values for hearing aid fitting based on audiometric data, patient age, and experience with amplification. This formula is implemented in the PrescriptiveTargetCalculationService and produces FittingPrescription value objects for adult fittings.

### DSL v5

The Desired Sensation Level version 5 (DSL v5) prescriptive formula provides pediatric-specific fitting targets that account for the acoustics of small ear canals and the different listening needs of children. This formula is the mandated standard for pediatric fitting in the Pediatric Audiology Context.

## Standards Integration in the Domain Model

Standards are integrated into the domain model at three levels. At the value object level, standards define valid ranges and validation rules (e.g., valid audiometric frequencies per ANSI S3.6). At the domain service level, standards define calculation algorithms (e.g., NAL-NL2 target computation). At the business rule level, standards define workflow constraints (e.g., EHDI timelines from JCIH). This layered integration ensures that standards compliance is enforced throughout the model rather than checked only at system boundaries.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- American National Standards Institute (ANSI). S3.6 - Specification for Audiometers.
- American National Standards Institute (ANSI). S3.1 - Maximum Permissible Ambient Noise Levels.
- American Speech-Language-Hearing Association (ASHA). Scope of Practice in Audiology.
- American Academy of Audiology (AAA). Clinical Practice Guidelines.
- Joint Committee on Infant Hearing (JCIH). (2019). Year 2019 Position Statement.
