# Anti-Corruption Layer in the Urology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from the concepts, terminology, and data structures of external systems. In the urology domain, anti-corruption layers are essential because urology systems must integrate with pathology, radiology, pharmacy, EHR, and laboratory information systems, each with its own data model and terminology that does not align directly with the urology domain model.

## Purpose

Without an ACL, external system concepts leak into the urology domain model, corrupting its integrity. For example, a pathology system may represent a prostate biopsy result as a generic "specimen report" with unstructured text fields, while the Oncologic Urology Context needs a structured Gleason score with primary pattern, secondary pattern, ISUP Grade Group, number of positive cores, percentage of involvement, and perineural invasion status. The ACL translates between these representations.

## ACL Between Oncologic Context and Pathology

The Oncologic Urology Context receives pathology reports for biopsies, surgical specimens, and cytology. The ACL translates generic pathology data structures into oncologic domain objects. A pathology report containing narrative text about "adenocarcinoma, Gleason 3+4=7" is parsed and translated into a structured GleasonScore value object with primaryPattern=3, secondaryPattern=4, isupGradeGroup=2, and associated PrognosticStageGroup. The ACL also handles variations in pathology reporting formats across different laboratory systems.

## ACL Between Clinical Assessment and Radiology

The Clinical Assessment Context receives imaging reports from radiology information systems. Radiology reports follow their own structured reporting standards (BI-RADS for breast, PI-RADS for prostate, Bosniak for renal cysts). The ACL translates radiology-specific classifications into the Clinical Assessment context's diagnostic model. A PI-RADS 4 lesion in a radiology report becomes a ProstateLesion entity with a suspicionLevel of "clinically significant cancer likely" in the clinical assessment model.

## ACL Between Stone Disease and Laboratory

The Stone Disease Context receives 24-hour urine collection results and stone composition analysis from laboratory systems. Laboratory systems report results in generic lab value formats with reference ranges. The ACL translates these into MetabolicProfile value objects with clinically meaningful interpretations: hypercalciuria (>250 mg/day for women, >300 mg/day for men), hypocitraturia (<320 mg/day), hyperoxaluria (>40 mg/day), and corresponding stone risk categories.

## ACL Between All Contexts and EHR

Every bounded context interfaces with the EHR system through an ACL. The EHR represents patients as generic clinical entities with demographics, problem lists, medication lists, and encounter records. Each urology context translates the EHR patient into its own domain-specific patient model. The Clinical Assessment Context's patient includes urological symptom history and diagnostic test results. The Oncologic Context's patient includes cancer history, staging timeline, and treatment response trajectory.

## ACL Between Incontinence Context and Pharmacy

The Incontinence Management Context manages pharmacotherapy including antimuscarinics (oxybutynin, solifenacin), beta-3 agonists (mirabegron), and combination therapies. The pharmacy system uses NDC codes and generic drug identifiers. The ACL translates pharmacy data into the Incontinence context's medication model, which organizes drugs by mechanism of action, expected efficacy, side effect profile, and contraindications relevant to incontinence management.

## ACL Between Male Reproductive Health and Endocrine Lab

The Male Reproductive Health Context receives hormonal lab results (total testosterone, free testosterone, LH, FSH, estradiol, prolactin, SHBG). The laboratory system reports these as individual results with reference ranges. The ACL assembles them into a HormonalProfile aggregate that includes time-of-day validation (morning draw requirement), age-adjusted interpretation, and clinical syndrome classification (primary versus secondary hypogonadism).

## Implementation Patterns

ACLs in the urology domain typically employ adapter and facade patterns. An adapter converts external system interfaces into interfaces expected by the domain. A facade simplifies complex external system APIs into the specific operations the urology context needs. Translation services encapsulate the mapping logic, making it testable and maintainable independently of both the external system and the domain model.

## Bidirectional Translation

Some ACLs must translate in both directions. When the Surgical Management Context sends operative reports to the EHR, the ACL translates domain-specific surgical documentation (robotic port placement, nerve-sparing technique, estimated blood loss) into the EHR's clinical document format. This outbound translation is as important as inbound translation for maintaining model integrity.

## Versioning and Evolution

External systems change their data formats and APIs over time. The ACL provides a stable boundary that absorbs these changes without impacting the domain model. When a pathology system upgrades its reporting format, only the ACL needs to be updated. The domain model and its business logic remain untouched, reducing the risk and cost of external system changes.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9.
- Hohpe, Gregor and Woolf, Bobby. Enterprise Integration Patterns. Addison-Wesley, 2003.
