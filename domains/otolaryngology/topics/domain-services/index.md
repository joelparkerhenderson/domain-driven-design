# Domain Services - Otolaryngology Domain

## Overview

Domain services are stateless operations that encapsulate domain logic not naturally belonging to a single entity or value object. They represent actions or computations that span multiple aggregates or require coordination between domain objects. In the otolaryngology domain, domain services capture clinical decision-making processes, scoring algorithms, eligibility determinations, and cross-aggregate coordination that reflect how ENT specialists, audiologists, and other providers reason about clinical situations.

## Domain Service Characteristics

Domain services in the otolaryngology domain share these characteristics:

- **Stateless**: Domain services hold no state between invocations. All necessary data is passed as parameters or retrieved through repositories.
- **Domain language**: Service operations are named using clinical terminology. "EvaluateCochlearImplantCandidacy" is a domain service, not "ProcessCIReferral."
- **Cross-aggregate logic**: Services coordinate logic that spans multiple aggregates within a bounded context or requires information from multiple domain objects.
- **Pure business logic**: Domain services contain no infrastructure concerns. They do not access databases, send messages, or interact with external systems directly.

## Key Domain Services by Bounded Context

### Clinical Assessment Context

**DiagnosticCorrelationService**
- Purpose: Correlates findings from multiple examination modalities (physical examination, endoscopy, audiometry, imaging) to suggest differential diagnoses.
- Operations: `correlateFindings(examinationFindings, endoscopicFindings, audiometricResults, imagingResults)` returns a list of ranked differential diagnoses with supporting evidence from each modality.
- Domain logic: Applies clinical reasoning rules that connect patterns of findings to diagnostic categories (e.g., unilateral conductive hearing loss plus tympanometric type B plus middle ear effusion on CT suggests chronic otitis media with effusion).

**ReferralDecisionService**
- Purpose: Determines appropriate subspecialty referrals based on clinical assessment findings.
- Operations: `determineReferrals(encounter)` analyzes encounter findings and generates referral recommendations to subspecialty contexts (voice, sleep, allergy, hearing, surgery).
- Domain logic: Applies evidence-based referral criteria (e.g., AHI >5 with symptoms triggers Sleep Disorders referral; vocal fold lesion triggers Voice Disorders referral).

### Surgical Management Context

**SurgicalEligibilityService**
- Purpose: Evaluates whether a patient meets criteria for a specific surgical procedure.
- Operations: `evaluateEligibility(patientId, procedureType, preOperativeAssessment)` returns an eligibility determination with any outstanding requirements.
- Domain logic: Applies procedure-specific eligibility criteria (e.g., septoplasty requires documented nasal obstruction and failed medical therapy; cochlear implant requires documented hearing aid trial failure).

**SurgicalRiskAssessmentService**
- Purpose: Calculates composite surgical risk based on patient factors and procedure characteristics.
- Operations: `assessRisk(patientFactors, procedureType)` returns a structured risk assessment including airway risk, bleeding risk, and anesthesia risk.
- Domain logic: Combines ASA classification, airway assessment (Mallampati, thyromental distance), anticoagulation status, and procedure-specific risk factors.

### Voice Disorders Context

**VoiceOutcomeScoringService**
- Purpose: Calculates and interprets standardized voice outcome measures.
- Operations: `calculateVHI(responses)` computes the Voice Handicap Index from patient responses. `calculateGRBAS(ratings)` computes the perceptual voice rating. `compareOutcomes(preAssessment, postAssessment)` determines clinically significant change.
- Domain logic: Applies validated scoring algorithms and clinically significant difference thresholds (e.g., VHI change of 18 points is clinically significant).

**VoiceTherapyCandidacyService**
- Purpose: Determines whether a patient is a candidate for voice therapy versus surgical intervention.
- Operations: `evaluateCandidacy(voiceAssessment)` returns a treatment recommendation (therapy, surgery, or combined) with supporting rationale.
- Domain logic: Applies clinical guidelines for voice therapy candidacy based on diagnosis, lesion characteristics, voice demands, and patient preferences.

### Sleep Disorders Context

**SleepApneaSeverityClassificationService**
- Purpose: Classifies sleep apnea severity from polysomnography data and determines appropriate treatment pathways.
- Operations: `classifySeverity(polysomnographyResult)` returns severity classification and recommended treatment pathway. `assessCPAPFailure(complianceRecords, symptoms)` determines whether CPAP therapy has failed.
- Domain logic: Applies AHI thresholds (mild 5-15, moderate 15-30, severe >30), positional versus non-positional classification, and REM-related versus non-REM classification. CPAP failure criteria include usage <4 hours/night for >70% of nights over 90 days.

### Allergy Management Context

**AllergenSensitizationProfileService**
- Purpose: Analyzes allergy test results to build a comprehensive sensitization profile and determine immunotherapy candidacy.
- Operations: `buildProfile(skinTestResults, serumResults)` creates a unified sensitization profile. `evaluateImmunotherapyCandidacy(profile, clinicalHistory)` determines immunotherapy eligibility.
- Domain logic: Correlates skin test and serum results, resolves discrepancies, classifies by allergen category (perennial, seasonal, occupational), and applies immunotherapy candidacy criteria.

### Hearing Services Context

**HearingLossClassificationService**
- Purpose: Classifies hearing loss type, degree, and configuration from audiometric data.
- Operations: `classifyHearingLoss(audiogram)` returns type (conductive, sensorineural, mixed), degree (mild through profound), and configuration (flat, sloping, rising, notched). `assessAsymmetry(leftAudiogram, rightAudiogram)` evaluates interaural asymmetry.
- Domain logic: Applies standard audiological classification criteria. Asymmetry assessment follows AAO-HNS criteria for retrocochlear evaluation referral.

**CochlearImplantCandidacyService**
- Purpose: Evaluates cochlear implant candidacy based on audiological, medical, and psychosocial criteria.
- Operations: `evaluateCandidacy(audiologicalRecord, medicalHistory, hearingAidTrialResults)` returns a candidacy determination with supporting criteria.
- Domain logic: Applies FDA-approved candidacy criteria, including bilateral severe-to-profound sensorineural hearing loss, limited benefit from appropriately fitted hearing aids (sentence recognition scores), and absence of medical contraindications.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain services.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 5 on domain services.
