# Voice Disorders Context - Otolaryngology Domain

## Overview

The Voice Disorders Context manages the evaluation and treatment of voice, speech, and swallowing disorders within the otolaryngology domain. It operates at the intersection of otolaryngology and speech-language pathology, requiring close collaboration between laryngologists and speech therapists. This context captures the specialized workflows for laryngoscopic examination, objective and subjective voice measurement, voice therapy protocol management, and phonosurgical planning.

## Subdomain Classification

Voice Disorders is a supporting subdomain. While it requires custom domain modeling to capture the relationship between laryngoscopic findings and treatment decisions, it follows well-established evaluation and treatment protocols documented in clinical practice guidelines. The Voice Handicap Index, GRBAS scale, and voice therapy approaches are standardized across the profession.

## Ubiquitous Language

- **Voice Assessment**: A comprehensive evaluation combining laryngoscopic findings, acoustic measurements, aerodynamic measurements, and patient-reported outcomes.
- **Laryngoscopic Examination**: Visualization of the larynx and vocal folds using rigid or flexible endoscopy, with or without stroboscopy.
- **Voice Handicap Index (VHI)**: A 30-item patient-reported outcome measure quantifying functional, physical, and emotional impact of voice disorders.
- **Vocal Fold Pathology**: Structural or functional abnormality of the vocal folds including nodules, polyps, cysts, granulomas, leukoplakia, and paralysis.
- **Voice Therapy Protocol**: A structured program of behavioral exercises prescribed by a speech-language pathologist to rehabilitate disordered voice production.
- **Phonosurgery**: Surgical procedures on the larynx to improve or restore voice function, including microlaryngoscopy, injection laryngoplasty, and thyroplasty.

## Aggregate: Voice Assessment

The Voice Assessment is the primary aggregate root, maintaining consistency across all components of voice evaluation and treatment planning.

### Entities Within the Aggregate

- **Laryngoscopic Examination**: Records vocal fold appearance, mobility, vibratory characteristics (stroboscopy), and supraglottic behavior.
- **Voice Therapy Protocol**: Tracks therapy goals, session schedule, exercises prescribed, progress notes, and outcome measurements.
- **Phonosurgery Plan**: Documents surgical indication, planned technique, expected outcomes, and pre-operative voice measurements for comparison.

### Key Value Objects

- **Voice Handicap Index Score**: Total score with functional, physical, and emotional subscale scores and severity classification.
- **GRBAS Scale Rating**: Perceptual voice quality rating for grade, roughness, breathiness, asthenia, and strain (each 0-3).
- **Acoustic Measurement**: Fundamental frequency, jitter, shimmer, and noise-to-harmonics ratio.
- **Aerodynamic Measurement**: Maximum phonation time, subglottic pressure estimate, airflow rate.
- **Stroboscopic Finding**: Mucosal wave, glottic closure pattern, phase symmetry, amplitude, and periodicity.
- **Vocal Fold Pathology Type**: Classification (nodule, polyp, cyst, granuloma, papilloma, leukoplakia, paralysis, paresis).

### Invariants

- A voice assessment must include at least one objective measure (acoustic or aerodynamic) and one subjective measure (VHI or GRBAS).
- Therapy recommendations must be supported by documented assessment findings.
- A phonosurgery plan requires pre-operative voice measurements to establish a baseline for outcome comparison.
- Voice therapy protocols must specify measurable goals and defined reassessment intervals.
- Outcome measurement must use the same instruments as the baseline assessment to enable valid comparison.

## Domain Events Published

- **VoiceAssessmentCompleted**: Notifies the system of a completed voice evaluation with findings summary and recommendations.
- **VoiceTherapyProtocolInitiated**: Notifies the system that a voice therapy program has begun.
- **VoiceTherapyOutcomeMeasured**: Publishes pre-treatment and post-treatment comparison with clinically significant change determination.

## Domain Events Consumed

- **EndoscopyPerformed** (from Clinical Assessment): Receives laryngoscopic findings from general ENT encounters for patients with voice complaints.
- **SurgeryCompleted** (from Surgical Management): Receives phonosurgery outcome information to plan post-surgical voice rehabilitation.

## Domain Services

- **VoiceOutcomeScoringService**: Calculates VHI scores, GRBAS ratings, and determines clinically significant change between assessments (VHI change of 18 points or more).
- **VoiceTherapyCandidacyService**: Evaluates whether a patient is a candidate for voice therapy, surgery, or combined treatment based on diagnosis, lesion characteristics, and voice demands.

## Clinical Workflow

1. Patient referred for voice evaluation (from Clinical Assessment or self-referral).
2. Laryngoscopic examination performed (vocal fold visualization and stroboscopy).
3. Objective voice measurements obtained (acoustic and aerodynamic analysis).
4. Patient completes Voice Handicap Index questionnaire.
5. Clinician performs perceptual voice rating (GRBAS).
6. Assessment synthesized and treatment plan determined (therapy, surgery, or combined).
7. If therapy: Voice therapy protocol created with goals, session schedule, and reassessment plan.
8. If surgery: Phonosurgery plan created with technique, expected outcomes, and post-operative rehabilitation.
9. Outcome measured at defined intervals using baseline assessment instruments.

## Integration Points

- **Clinical Assessment Context**: Receives laryngoscopic findings; shares voice evaluation results for comprehensive ENT documentation.
- **Surgical Management Context**: Coordinates phonosurgery planning and receives post-surgical status for voice rehabilitation.
- **Hearing Services Context**: Shares assessment data for patients with combined voice and hearing disorders through a shared kernel for communication assessment concepts.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Rosen, Clark A., and Thomas Murry. *Voice Therapy: Clinical Case Studies*. 4th ed., Plural Publishing, 2017.
- Jacobson, Barbara H., et al. "The Voice Handicap Index (VHI): Development and Validation." *American Journal of Speech-Language Pathology* 6.3 (1997): 66-70.
