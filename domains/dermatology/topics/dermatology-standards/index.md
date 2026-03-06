# Dermatology Standards

Dermatology standards encompass the classification systems, coding frameworks, clinical guidelines, and interoperability specifications that inform the domain model design. These standards serve as published languages and value object definitions within the dermatology domain, ensuring that clinical concepts are represented consistently and in alignment with industry-wide conventions. As Evans (2003) notes, domain-specific standards are a natural source of published languages and value object design.

## Diagnostic Classification Standards

### ICD-10 Dermatology Codes

The International Classification of Diseases, 10th Revision, provides the primary diagnostic coding framework for the dermatology domain. Chapter XII (L00-L99) covers diseases of the skin and subcutaneous tissue, organized into blocks: infections (L00-L08), dermatitis and eczema (L20-L30), papulosquamous disorders (L40-L45), urticaria and erythema (L50-L54), radiation-related disorders (L55-L59), disorders of skin appendages (L60-L75), and other disorders (L80-L99). Skin neoplasms are coded in Chapter II: melanoma (C43), other malignant neoplasms of skin (C44), carcinoma in situ (D04), and benign neoplasms (D22-D23).

In the domain model, ICD-10 codes are represented as value objects within the Clinical Assessment Context (for documented findings) and the Lesion Management Context (for confirmed diagnoses). The value object validates code format, verifies existence in the ICD-10 code set, and provides the hierarchical classification context.

### SNOMED CT

The Systematized Nomenclature of Medicine -- Clinical Terms provides a more granular clinical terminology than ICD-10. SNOMED CT concepts are used within the domain model for clinical specificity that ICD-10 codes cannot capture. For example, SNOMED CT distinguishes between different morphological subtypes of basal cell carcinoma (nodular, superficial, morpheaform, infiltrative) that map to different treatment approaches. SNOMED CT concepts serve as supplementary value objects alongside ICD-10 codes.

### Dermoscopy Terminology

The International Dermoscopy Society has standardized dermoscopic terminology through consensus publications. Standard terms for dermoscopic structures (pigment network, dots, globules, streaks, blue-white veil, regression structures, vascular patterns) are represented as value objects within the Clinical Assessment Context. The Third Consensus Conference on Dermoscopy (2016) established the current standard terminology that the domain model adopts.

## Procedural Coding Standards

### CPT Codes for Dermatology

Current Procedural Terminology codes identify specific dermatologic procedures for documentation, billing, and outcome tracking. Key dermatology CPT code ranges include: skin biopsy (11102-11107), excision of benign lesions (11400-11471), excision of malignant lesions (11600-11646), Mohs micrographic surgery (17311-17315), destruction of lesions by cryotherapy (17000-17004), phototherapy (96910-96913), and injectable treatments (various codes by product and site).

CPT codes are represented as value objects within the Procedure Management Context, associated with each Procedure Session aggregate. The value object carries the code, description, and applicable modifiers.

### HCPCS Codes

The Healthcare Common Procedure Coding System provides additional codes for supplies, drugs, and services not covered by CPT. In dermatology, HCPCS codes are relevant for injectable drug products (J codes for botulinum toxin, dermal fillers, and other therapeutic injections), durable medical equipment, and phototherapy equipment. These codes supplement CPT codes in the Procedure Management and Cosmetic Services Contexts.

## Clinical Scoring Standards

### PASI (Psoriasis Area and Severity Index)

The PASI scoring system, published by Fredriksson and Pettersson (1978), is the gold standard for assessing psoriasis severity. The scoring protocol divides the body into four regions (head, trunk, upper extremities, lower extremities), assigns area coverage percentages and intensity grades for erythema, induration, and desquamation, and produces a composite score from 0 to 72. The PASI calculation algorithm is encoded as a shared kernel value object used by both the Clinical Assessment Context and Treatment Outcomes Context. PASI 75 (75 percent reduction from baseline) is the standard efficacy endpoint in clinical trials.

### SCORAD (Scoring Atopic Dermatitis)

The SCORAD index, developed by the European Task Force on Atopic Dermatitis (1993), measures atopic dermatitis severity through three components: extent of affected body surface area (0-100 percent using the rule of nines), intensity (six clinical signs each scored 0-3), and subjective symptoms (pruritus and sleep loss each scored 0-10 on a visual analog scale). The composite score ranges from 0 to 103, with severity bands: mild (less than 25), moderate (25-50), and severe (greater than 50).

### DLQI (Dermatology Life Quality Index)

The DLQI, developed by Finlay and Khan (1994), is a ten-question patient-reported outcome measure assessing the impact of skin disease on quality of life. Questions cover daily activities, leisure, work, relationships, treatment burden, and emotional impact. Each question scores 0-3, producing a total from 0 to 30. Severity bands range from no effect (0-1) to extremely large effect (21-30).

## Staging Standards

### AJCC Staging for Cutaneous Malignancies

The American Joint Committee on Cancer staging system provides the TNM classification for skin cancers. For melanoma, the T category is determined by Breslow thickness and ulceration status. For cutaneous squamous cell carcinoma, staging considers tumor size, depth, perineural invasion, and anatomical site. The AJCC staging criteria are encoded in the StagingClassificationService within the Pathology Integration Context.

## Interoperability Standards

### HL7 and FHIR

Health Level Seven (HL7) v2 messaging and Fast Healthcare Interoperability Resources (FHIR) define the communication standards for the Pathology Integration Context's anti-corruption layer. HL7 ORM (order) and ORU (result) messages handle specimen ordering and result delivery. FHIR resources (DiagnosticReport, Specimen, Observation) provide a modern alternative for laboratory integration. The domain model translates between these interoperability formats and its internal representation.

### DICOM for Dermatology Imaging

The Digital Imaging and Communications in Medicine standard applies to clinical and dermoscopic photography storage and exchange. DICOM metadata tags provide standardized fields for anatomical location, modality, acquisition parameters, and patient demographics that the domain model can map to its internal value objects.

## Standards Governance

Standards evolve through revisions by their governing bodies (WHO for ICD, AMA for CPT, AJCC for staging). The domain model must be designed to accommodate standard updates without requiring fundamental model restructuring. Khononov (2021) recommends treating standards-based value objects as versioned components, enabling the system to support multiple standard versions during transition periods.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 14 on published languages.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- World Health Organization. *International Classification of Diseases, 10th Revision (ICD-10)*.
- American Medical Association. *Current Procedural Terminology (CPT)*.
- SNOMED International. *SNOMED CT Editorial Guide*.
- Kittler, Harald, et al. "Standardization of Dermoscopic Terminology." Archives of Dermatology, 2016.
