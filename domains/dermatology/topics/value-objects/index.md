# Value Objects in the Dermatology Domain

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal regardless of which instance is referenced. In the dermatology domain, value objects capture clinical measurements, classification codes, scoring results, anatomical references, and treatment parameters that are meaningful as composite values rather than as individually tracked entities. As Evans (2003) establishes, value objects simplify the model by eliminating identity management for concepts that do not require it.

## Characteristics in Dermatology

Dermatology value objects share the properties of immutability, structural equality, and self-validation. A PASI score of 12.4 is always equal to another PASI score of 12.4 regardless of when or where it was calculated. If a value needs to change, a new value object is created rather than modifying the existing one. Each value object validates its own construction, rejecting invalid values such as a negative Breslow thickness or a Fitzpatrick skin type outside the I-VI range.

## Clinical Assessment Value Objects

### Fitzpatrick Skin Type

Represents the patient's skin classification on the Fitzpatrick scale (types I through VI). The value object carries the type number and its associated characteristics (burn tendency, tan tendency, typical skin color). It validates that the type falls within the I-VI range and is immutable once assigned to a patient profile.

### Morphological Descriptor

Captures the clinical appearance characteristics of a lesion, including shape (round, oval, irregular), border quality (well-defined, diffuse, irregular), color (list of observed colors), surface texture (smooth, rough, scaly, ulcerated), and elevation (flat, raised, pedunculated). Each component is validated against accepted dermatological terminology.

### ABCDE Score

Represents the melanoma screening assessment as a composite value with individual component scores for Asymmetry, Border irregularity, Color variation, Diameter measurement, and Evolution assessment. Each component carries both a qualitative descriptor and a quantitative score. The composite provides a risk classification.

### Dermoscopic Pattern

Captures a specific dermoscopic pattern observation (reticular, globular, cobblestone, homogeneous, starburst, parallel, multicomponent, nonspecific, or lacunar). The value object carries the pattern type, confidence level, and any associated colors or structures observed.

### Anatomical Location

Represents a precise location on the body using a standardized anatomical reference system. The value object carries the body region (head, trunk, upper extremity, lower extremity), subregion (specific area such as left forearm, right temple), laterality (left, right, midline, bilateral), and optional coordinates on a body map. Two lesions at the same anatomical location produce equal value objects.

### Lesion Measurement

Captures the physical dimensions of a lesion as a composite value including length, width, and height (all in millimeters), estimated surface area, and measurement method (clinical, dermoscopic, or photographic). The value object validates that all measurements are positive numbers.

## Lesion Management Value Objects

### Surgical Margin

Represents the planned or achieved margin width around an excised lesion, measured in millimeters. The value object carries the recommended margin (based on diagnosis and guidelines), the planned margin, and the achieved margin (measured post-excision). It validates that margin values are non-negative.

### Follow-Up Interval

Captures the recommended time period between monitoring visits for a specific lesion, expressed as a duration (weeks, months, or years) with a risk-based justification. The value object includes the interval duration, the guideline source, and the risk category that determined the interval.

### Biopsy Type Classification

Represents the specific biopsy technique (shave, saucerization, punch with size in millimeters, incisional, or excisional) as a value object. It carries the technique identifier, applicable punch size if relevant, and depth expectation.

## Procedure Management Value Objects

### Cryotherapy Parameters

Captures the treatment parameters for a single cryotherapy application: freeze duration in seconds, thaw duration, number of freeze-thaw cycles, spray distance, and target tissue temperature. The value object validates that durations are positive and cycle count is within clinical ranges.

### Laser Settings

Represents the configuration of a laser device for a specific treatment pass: wavelength (nanometers), pulse duration (milliseconds), fluence (joules per square centimeter), spot size (millimeters), and repetition rate (hertz). The value object validates that all parameters fall within safe operating ranges for the specified laser type.

### Phototherapy Dose

Captures the ultraviolet light dose for a single phototherapy session: dose value (millijoules per square centimeter for UVB or joules per square centimeter for PUVA), light type (broadband UVB, narrowband UVB, or PUVA), and exposure duration. The value object enforces maximum dose limits based on protocol and cumulative exposure.

## Cosmetic Services Value Objects

### Injectable Dose

Represents the amount of an injectable product administered to a specific treatment area: product type (neuromodulator or filler category), product name, units or volume, and lot number. The value object validates dosage against maximum limits per treatment area.

### Treatment Area Specification

Captures a precisely defined anatomical zone for cosmetic treatment: facial region (forehead, glabella, periorbital, perioral, jawline, nasolabial), laterality, and reference landmarks. This value object enables consistent documentation of injection sites across sessions.

## Pathology Value Objects

### TNM Classification

Represents the tumor-node-metastasis staging for a cutaneous malignancy. The value object carries the T category (tumor size and depth), N category (lymph node involvement), M category (distant metastasis), and the derived overall stage (0, I, II, III, IV). Breslow thickness (millimeters) and Clark level (I-V) are included for melanoma. The value object validates staging consistency.

### ICD-10 Dermatology Code

Represents a diagnostic code from ICD-10 Chapter XII (L00-L99) or other relevant chapters for skin neoplasms. The value object carries the code, description, and category hierarchy, validating code format and existence.

## Treatment Outcomes Value Objects

### PASI Score

Represents a calculated Psoriasis Area and Severity Index score as a composite value: head, trunk, upper extremity, and lower extremity subscores, each comprising area percentage and intensity measures (erythema, induration, desquamation). The composite score ranges from 0 to 72. The value object validates all component ranges and performs the standardized calculation.

### SCORAD Index

Captures the Scoring Atopic Dermatitis assessment: extent percentage (0-100), intensity score (0-18 from six items), and subjective symptom scores (0-20 for pruritus and sleep loss). The composite ranges from 0 to 103.

### DLQI Score

Represents the Dermatology Life Quality Index result: ten individual question scores (0-3 each) and the total score (0-30), with severity banding (no effect, small, moderate, very large, extremely large effect on quality of life).

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013. Chapter 6 on value object design and implementation.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021. Chapter 7 on value objects within aggregates.
