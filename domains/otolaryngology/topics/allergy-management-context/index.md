# Allergy Management Context - Otolaryngology Domain

## Overview

The Allergy Management Context handles allergic disease evaluation and treatment within the otolaryngology scope. It covers the full spectrum of ENT allergy care, from diagnostic testing (skin prick testing, intradermal testing, serum-specific IgE measurement) through treatment (pharmacotherapy, allergen immunotherapy) to chronic disease management for allergic rhinitis and chronic rhinosinusitis. This context captures the specialized workflows of ENT allergists who manage patients with upper airway allergic conditions.

## Subdomain Classification

Allergy Management is a supporting subdomain. Allergy testing and immunotherapy follow standardized protocols published in practice parameters by the Joint Task Force on Practice Parameters (representing AAAAI and ACAAI). While the relationship between sensitization patterns and individualized treatment plans requires custom modeling, the underlying testing protocols, immunotherapy dosing schedules, and safety monitoring requirements are well-established.

## Ubiquitous Language

- **Allergy Profile**: The primary aggregate representing a patient's complete allergy sensitization pattern and treatment history.
- **Skin Prick Test**: An in-vivo test where allergen extracts are introduced into the epidermis to assess immediate (Type I) hypersensitivity.
- **Intradermal Test**: A more sensitive in-vivo test where allergen extract is injected into the dermis, used for allergens negative on prick testing.
- **Allergen Panel**: A defined set of allergen extracts used in diagnostic testing, organized by category (trees, grasses, weeds, molds, dust mites, animal dander).
- **Immunotherapy Protocol**: A structured treatment plan of escalating allergen doses to induce immune tolerance.
- **Allergic Rhinitis**: IgE-mediated nasal mucosal inflammation caused by environmental allergens.
- **Chronic Rhinosinusitis (CRS)**: Persistent sinonasal inflammation lasting twelve or more weeks, which may be with or without nasal polyps.

## Aggregate: Allergy Profile

The Allergy Profile is the primary aggregate root, maintaining a longitudinal record of a patient's allergic sensitizations and treatments.

### Entities Within the Aggregate

- **Skin Test Session**: Records a complete allergy skin testing session including positive and negative controls, individual allergen results, and overall sensitization interpretation.
- **Serum IgE Test**: Records laboratory serum-specific IgE results with allergen, concentration, class, and interpretation.
- **Immunotherapy Protocol**: Tracks the full immunotherapy treatment course including allergen extract formulation, dosing schedule, current phase, dose history, and reaction documentation.
- **Treatment Plan**: Documents current pharmacotherapy, avoidance strategies, and management goals for allergic rhinitis or chronic rhinosinusitis.

### Key Value Objects

- **Skin Test Result**: Allergen name, category, wheal diameter (mm), flare diameter (mm), interpretation (negative, 1+ to 4+).
- **Serum IgE Level**: Allergen, concentration (kU/L), class (0-6), interpretation (negative to very high).
- **Immunotherapy Dose**: Allergen extract, concentration, volume, injection site, observed reaction type and severity.
- **Allergen Sensitivity**: Allergen name, category (perennial, seasonal), sensitization level, clinical relevance.
- **Rhinitis Severity Score**: Congestion, rhinorrhea, sneezing, and itching subscores with total and classification.
- **Lund-Mackay CT Score**: Sinus-by-sinus CT staging score (0-24) for chronic rhinosinusitis severity.

### Invariants

- An immunotherapy protocol cannot be initiated without documented allergen sensitivities from testing.
- Allergen extract formulations for immunotherapy must correspond to documented positive test results.
- Dose escalation in immunotherapy requires documented tolerance of the previous dose.
- Immunotherapy dose reduction rules apply after missed appointments exceeding defined time thresholds.
- Surgical referral for chronic rhinosinusitis requires documentation of failed maximal medical therapy.
- Skin testing sessions must include valid positive (histamine) and negative (saline) controls.

## Domain Events Published

- **AllergyTestingCompleted**: Publishes test results and sensitization summary.
- **ImmunotherapyProtocolStarted**: Notifies the system of new immunotherapy initiation.
- **ImmunotherapyReactionRecorded**: Publishes adverse reaction details for safety monitoring.
- **SinusitisSurgeryReferred**: Notifies Surgical Management of a patient referred for sinus surgery after failed medical therapy.

## Domain Events Consumed

- **EndoscopyPerformed** (from Clinical Assessment): Receives nasal endoscopy findings (polyps, mucosal edema, purulent drainage) for correlation with allergy findings.
- **SurgeryCompleted** (from Surgical Management): Receives post-surgical status for ongoing medical management of CRS after FESS.

## Domain Services

- **AllergenSensitizationProfileService**: Analyzes test results to build comprehensive sensitization profiles, correlate skin and serum results, classify by allergen category, and determine immunotherapy candidacy.

## Clinical Workflow

1. Patient presents with nasal or sinus complaints suggestive of allergic etiology.
2. Allergy history obtained and nasal examination performed.
3. Allergy testing ordered (skin prick testing for initial screen, intradermal for selected allergens, serum IgE as alternative).
4. Testing results interpreted and sensitization profile built.
5. Treatment plan formulated (avoidance, pharmacotherapy, immunotherapy, or combination).
6. If immunotherapy indicated: Protocol created with allergen formulation matching sensitivities.
7. Immunotherapy build-up phase with escalating doses and safety monitoring.
8. Maintenance phase with regular injections at target dose.
9. For CRS patients: Medical therapy trial documented; surgical referral if refractory.
10. Outcome assessment at defined intervals (symptom scores, medication usage).

## Integration Points

- **Clinical Assessment Context**: Receives nasal endoscopy findings for correlation with allergy test results.
- **Sleep Disorders Context**: Shares nasal obstruction data for patients with concurrent allergic rhinitis and sleep-disordered breathing.
- **Surgical Management Context**: Refers medically refractory CRS patients for FESS with documented failed therapy.
- **Laboratory Information System**: Receives serum-specific IgE results through an anti-corruption layer.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- Joint Task Force on Practice Parameters. "Allergen Immunotherapy: A Practice Parameter Third Update." *Journal of Allergy and Clinical Immunology* 127.1 (2011): S1-S55.
- Rosenfeld, Richard M., et al. "Clinical Practice Guideline (Update): Adult Sinusitis." *Otolaryngology-Head and Neck Surgery* 152.S2 (2015): S1-S39.
