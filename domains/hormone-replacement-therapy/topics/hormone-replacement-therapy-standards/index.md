# Hormone Replacement Therapy Standards

Clinical and pharmacological standards inform the design of domain models, value objects, and published languages within the hormone replacement therapy (HRT) domain. These standards provide the authoritative definitions for laboratory reference ranges, dosing guidelines, contraindication lists, assessment instruments, and outcome measures that the domain model encodes.

## Purpose

Standards are critical in the HRT domain because clinical decision-making must align with evidence-based guidelines and professional consensus. Domain models that deviate from established standards risk producing clinically inappropriate recommendations. Value objects such as TherapeuticTargetRange, Dosage, and ContraindicationClassification derive their valid ranges and classifications from published standards. Published languages between bounded contexts must express data in formats that conform to healthcare interoperability standards.

## Clinical Practice Guidelines

### Endocrine Society Guidelines

The Endocrine Society publishes evidence-based clinical practice guidelines that directly inform multiple aspects of the HRT domain model. The "Testosterone Therapy in Men with Hypogonadism" guideline (Bhasin et al., 2018) defines diagnostic thresholds for testosterone deficiency, recommended initial dosing ranges for various delivery methods, monitoring parameters and schedules, and contraindications. The "Treatment of Symptoms of the Menopause" guideline (Stuenkel et al., 2015) provides similar authoritative guidance for estrogen and progesterone therapy.

### North American Menopause Society Position Statements

NAMS publishes regularly updated position statements on hormone therapy that reflect current clinical consensus. The 2022 Position Statement provides guidance on patient selection criteria, preferred formulations and delivery methods, risk-benefit assessment frameworks, and duration of therapy considerations. These position statements directly inform the CandidacyEvaluationService and ProtocolSelectionService logic.

### American Urological Association Guidelines

For male hormone therapy, AUA guidelines provide standards for testosterone deficiency diagnosis, treatment initiation criteria, monitoring protocols, and safety parameters including PSA monitoring and hematocrit thresholds.

## Laboratory Standards

### Reference Ranges

Laboratory reference ranges for hormone measurements are established by professional organizations and laboratory accreditation bodies. Key reference ranges used in the domain include estradiol (follicular phase, luteal phase, postmenopausal, and HRT therapeutic ranges), total and free testosterone (age-stratified male and female ranges), FSH and LH (menstrual cycle phase-dependent ranges), progesterone (luteal phase and supplementation ranges), and thyroid hormones (TSH, free T3, free T4 with assay-specific ranges).

### LOINC Coding

The Lab Monitoring Context uses LOINC (Logical Observation Identifiers Names and Codes) as the standard coding system for laboratory test identification. LOINC codes provide unambiguous identification of test types, enabling consistent communication between the domain and external laboratory information systems. The anti-corruption layer maps between LOINC codes and internal domain test identifiers.

### Units of Measurement

Hormone measurements use standardized units defined by the International System of Units (SI) and conventional clinical units. The domain must handle both SI units (pmol/L for estradiol, nmol/L for testosterone) and conventional units (pg/mL for estradiol, ng/dL for testosterone) with accurate conversion. The LabResult value object includes the unit of measurement and the HormoneLevel value object supports unit conversion.

## Pharmacological Standards

### FDA Drug Labeling

Approved HRT medications carry FDA-approved labeling that specifies indications, contraindications, dosing ranges, warnings, and adverse event profiles. The FormulationSpecification value object in the Hormone Protocol Context references FDA labeling data for each approved formulation. The ContraindicationRegistry in the Side Effect Management Context aligns with FDA-mandated warnings.

### USP Compounding Standards

For compounded hormone preparations, the United States Pharmacopeia (USP) provides standards for compounding quality, potency testing, and beyond-use dating. The domain model distinguishes between FDA-approved manufactured formulations and USP-regulated compounded preparations, as they have different quality assurance and regulatory oversight characteristics.

### NDC Coding

The National Drug Code (NDC) system provides unique identifiers for manufactured hormone preparations. The Hormone Protocol Context uses NDC codes in its anti-corruption layer when integrating with pharmacy systems, mapping between internal formulation identifiers and NDC codes.

## Assessment Instrument Standards

### Menopause Rating Scale (MRS)

The MRS is a validated instrument with standardized scoring that the Clinical Assessment Context uses for menopausal symptom evaluation. Its scoring algorithm, severity classifications, and normative data are published standards that the SymptomScoringService implements precisely.

### ADAM Questionnaire

The Androgen Deficiency in Aging Males (ADAM) questionnaire is a validated screening instrument for male androgen deficiency. Its questions and scoring criteria are published standards implemented in the assessment context.

### SF-36 and MENQOL

The SF-36 and MENQOL are validated quality of life instruments with standardized scoring algorithms, normative data, and minimal clinically important difference thresholds. The Outcomes Tracking Context implements these scoring standards precisely to ensure meaningful outcome measurement.

## Adverse Event Reporting Standards

### MedDRA Terminology

The Medical Dictionary for Regulatory Activities (MedDRA) provides the standard terminology for adverse event classification. The EventClassification value object in the Side Effect Management Context uses MedDRA system organ classes and preferred terms.

### CIOMS Reporting

The Council for International Organizations of Medical Sciences (CIOMS) defines international adverse event reporting formats. The anti-corruption layer in the Side Effect Management Context translates internal records to CIOMS format for international reporting.

## Healthcare Interoperability Standards

### HL7 FHIR

HL7 FHIR (Fast Healthcare Interoperability Resources) provides standard resource definitions for clinical data exchange. Anti-corruption layers use FHIR resource mappings for MedicationRequest (prescriptions), DiagnosticReport (lab results), and AdverseEvent (safety reports).

### HL7 v2

Legacy laboratory systems often communicate using HL7 v2 messaging, particularly ORU (Observation Result) messages. The Lab Monitoring Context anti-corruption layer includes HL7 v2 message parsing.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021.
- Bhasin, S., et al. "Testosterone Therapy in Men with Hypogonadism." JCEM, 2018.
- Stuenkel, C.A., et al. "Treatment of Symptoms of the Menopause." JCEM, 2015.
- North American Menopause Society. "The 2022 Hormone Therapy Position Statement." Menopause, 2022.
- Benson, Tim and Grieve, Grahame. Principles of Health Interoperability: FHIR, HL7 and SNOMED CT. Springer, 2021.
