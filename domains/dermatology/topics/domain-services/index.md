# Domain Services in the Dermatology Domain

Domain services are stateless operations that encapsulate domain logic which does not naturally belong to a single entity or value object. In the dermatology domain, domain services handle operations that span multiple aggregates, coordinate complex workflows, or implement business rules that require information from multiple domain objects. As Evans (2003) describes, domain services are defined in terms of the ubiquitous language and operate on domain concepts, distinguishing them from application services or infrastructure services.

## Characteristics of Dermatology Domain Services

Domain services in the dermatology domain are stateless, meaning they do not hold any data between invocations. They operate on domain objects passed as parameters and return domain objects or domain-meaningful results. They are named using verbs from the ubiquitous language and express operations that domain experts recognize as significant. They reside within a single bounded context and do not cross context boundaries directly.

## Clinical Assessment Context Domain Services

### LesionRiskAssessmentService

This domain service evaluates the risk level of a lesion finding based on multiple factors that span several value objects. The service takes a Lesion Finding entity with its Morphological Descriptor, ABCDE Score, Dermoscopic Pattern, and the patient's Fitzpatrick Skin Type, and produces a risk classification (low, moderate, high, or urgent). The assessment logic combines scoring algorithms with clinical rules such as heightened risk for specific dermoscopic patterns in fair-skinned patients, or accelerated concern when lesion evolution is documented over successive examinations. This logic does not belong to any single value object because it requires correlation across multiple domain concepts.

### DermoscopyPatternAnalysisService

This domain service analyzes a set of dermoscopic observations to suggest likely differential diagnoses. Given a collection of Dermoscopic Pattern value objects observed for a single lesion, the service applies pattern-matching rules derived from dermoscopic algorithms (pattern analysis method, ABCD rule of dermoscopy, Menzies method) to rank possible diagnoses. The service encapsulates the clinical reasoning that considers pattern combinations rather than individual patterns in isolation.

## Lesion Management Context Domain Services

### BiopsyIndicationService

This domain service evaluates whether a lesion meets criteria for biopsy based on its clinical characteristics, patient history, and applicable clinical guidelines. The service takes a Lesion Record with its assessment history and a reference to the Patient Skin Profile, and produces a biopsy recommendation with indication category, recommended biopsy type, and urgency level. The logic spans the lesion's morphological data, temporal evolution, patient risk factors, and guideline-based decision rules.

### MarginCalculationService

This domain service determines the recommended surgical margin for a lesion excision based on the confirmed diagnosis, tumor characteristics (such as Breslow thickness for melanoma), anatomical location, and applicable clinical guidelines (such as National Comprehensive Cancer Network guidelines for cutaneous malignancies). The service takes a Lesion Record with confirmed diagnosis and staging information and produces a Surgical Margin value object with the guideline-recommended width.

### FollowUpIntervalDeterminationService

This domain service calculates the appropriate follow-up interval for a treated lesion based on diagnosis, treatment modality, margin status, and risk stratification. The service applies guideline-based algorithms to determine how frequently a patient should return for monitoring, producing a Follow-Up Interval value object with the recommended duration and the guideline source.

## Procedure Management Context Domain Services

### PhototherapyDoseEscalationService

This domain service calculates the next phototherapy dose based on the patient's current protocol, previous session doses, cumulative exposure, skin response to previous treatments, and any adverse reactions reported. The service enforces dose escalation rules that depend on the relationship between the current dose, the minimum erythema dose, and the protocol's escalation schedule. This logic spans the Phototherapy Plan aggregate and its session history.

### ProcedureEligibilityService

This domain service evaluates whether a patient is eligible for a specific procedure based on the patient's medical history, current medications (particularly anticoagulants and photosensitizing drugs), skin type, and procedure-specific contraindications. The service takes patient reference data and the proposed procedure type and returns an eligibility assessment with any identified contraindications.

## Cosmetic Services Context Domain Services

### InjectableDosageCalculationService

This domain service calculates recommended injectable dosages based on the treatment area, patient anatomy, previous treatment history, and product-specific guidelines. For neuromodulators, the service considers muscle mass, prior response, and time since last treatment. For dermal fillers, the service considers tissue depth, volume loss assessment, and product rheological properties. The service produces an Injectable Dose value object with the recommended units or volume.

### CosmeticConsentValidationService

This domain service validates that all consent requirements are met before a cosmetic procedure can proceed. The service checks that a valid, non-expired consent exists for the specific procedure type, that the patient has been informed of relevant updates since consent was signed, and that any mandatory waiting periods between consultation and treatment have been observed.

## Pathology Integration Context Domain Services

### StagingClassificationService

This domain service determines the TNM staging for a cutaneous malignancy based on the histopathology report findings. The service takes pathology measurements (Breslow thickness, Clark level, ulceration status, mitotic rate) and any clinical findings (lymph node status, imaging results) and produces a TNM Classification value object with the derived stage. The staging logic follows AJCC staging guidelines and requires correlation across multiple data points that no single entity owns.

## Treatment Outcomes Context Domain Services

### TreatmentResponseEvaluationService

This domain service evaluates treatment response by comparing sequential severity assessment scores for a specific treatment. The service takes a series of PASI, SCORAD, or DLQI scores associated with a treatment and classifies the response (complete response: score reduction greater than 90 percent; partial response: 50-90 percent reduction; minimal response: 25-50 percent reduction; no response: less than 25 percent reduction; worsening: score increase). The evaluation logic spans multiple assessment records and requires percentage change calculation relative to baseline.

### OutcomeComparisonService

This domain service compares treatment outcomes across different treatment modalities for similar diagnoses. The service takes a set of Treatment Outcome Records with the same diagnosis category and compares response rates, time to response, and recurrence rates. This cross-aggregate analysis provides evidence for treatment protocol refinement.

## Service Design Guidelines

Vernon (2013) emphasizes that domain services should be used sparingly. Logic that can be expressed within a single entity or value object should remain there. Domain services are reserved for operations that genuinely require coordination across multiple domain objects. Khononov (2021) adds that domain services should be distinguished from application services (which orchestrate workflows) and infrastructure services (which handle technical concerns).

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on domain services.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 7 on domain service design.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 8 on domain services within bounded contexts.
- National Comprehensive Cancer Network. *NCCN Clinical Practice Guidelines in Oncology: Melanoma*.
