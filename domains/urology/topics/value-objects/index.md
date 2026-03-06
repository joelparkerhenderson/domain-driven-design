# Value Objects in the Urology Domain

## Overview

A value object is an immutable domain object defined by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal. In the urology domain, value objects represent clinical measurements, scores, classifications, and other concepts where the specific values matter but individual identity does not. Value objects are fundamental to expressing the precise clinical data that drives urological decision-making.

## Characteristics

Value objects in the urology domain are immutable: once created, their attributes do not change. If a measurement needs to be corrected, a new value object is created rather than modifying the existing one. This immutability provides safety in concurrent systems and ensures that historical clinical data is never inadvertently altered. Value objects also encapsulate validation rules, ensuring that only clinically valid combinations of attributes can exist.

## Clinical Assessment Value Objects

### IPSSScore

The IPSSScore value object captures the International Prostate Symptom Score. It contains seven symptom subscores (0-5 each) and one quality-of-life score (0-6), producing a total score of 0-35. The value object enforces range validation and provides severity classification: mild (0-7), moderate (8-19), or severe (20-35). Two IPSSScore instances with the same subscores are considered equal regardless of when or by whom they were recorded.

### BloodPressureReading

The BloodPressureReading value object captures systolic and diastolic values in mmHg, relevant for pre-operative assessment and hypertension management in stone disease (thiazide therapy). It enforces that systolic must exceed diastolic and both must be within physiologically plausible ranges.

### UrinaryFlowRate

The UrinaryFlowRate value object captures peak flow rate (Qmax) in mL/s, average flow rate in mL/s, voided volume in mL, and post-void residual volume in mL. It enforces that Qmax cannot exceed a physiological maximum and classifies obstruction severity based on the nomogram relationship between flow rate and voided volume.

## Oncologic Value Objects

### GleasonScore

The GleasonScore value object contains the primary pattern (3, 4, or 5), secondary pattern (3, 4, or 5), and computed sum (6-10). It also derives the ISUP Grade Group (1-5) according to the 2014 International Society of Urological Pathology consensus. The value object enforces that patterns must be between 3 and 5 for contemporary grading, and provides methods for risk interpretation.

### TNMStage

The TNMStage value object represents a cancer's anatomic extent. It contains the T (tumor) component, N (node) component, and M (metastasis) component, each following the AJCC/UICC classification rules for the specific cancer type (prostate, bladder, kidney, testicular). The value object derives the overall stage grouping (I, II, III, IV) from its components.

### PSAMeasurement

The PSAMeasurement value object captures a serum PSA level in ng/mL, the measurement date, and the assay method. It provides interpretation methods including age-adjusted reference ranges, PSA density (when prostate volume is provided), and free-to-total PSA ratio. Two PSA measurements taken on different dates are distinct value objects even if the numeric value is identical.

### PSAKinetics

The PSAKinetics value object is computed from a series of PSAMeasurement values. It contains PSA velocity (ng/mL/year) and PSA doubling time (months). It enforces a minimum of three measurements spanning at least 18 months for reliable kinetic calculation.

## Stone Disease Value Objects

### StoneCharacterization

The StoneCharacterization value object describes a kidney stone's physical properties: maximum dimension in mm, volume in mm3, density in Hounsfield units, location (calyceal, renal pelvis, ureteropelvic junction, ureter, bladder), and composition (calcium oxalate, calcium phosphate, uric acid, strite, cystine). It provides treatment suitability methods (ESWL candidacy based on size and density).

### TwentyFourHourUrineResult

The TwentyFourHourUrineResult value object captures the full metabolic panel from a 24-hour urine collection. It contains volume (mL), pH, calcium (mg/day), oxalate (mg/day), citrate (mg/day), uric acid (mg/day), sodium (mEq/day), magnesium (mg/day), phosphorus (mg/day), creatinine (mg/day), and supersaturation indices. It provides metabolic abnormality classifications: hypercalciuria, hyperoxaluria, hypocitraturia, hyperuricosuria, low volume.

## Incontinence Value Objects

### PadWeightResult

The PadWeightResult value object captures the result of a standardized pad weight test. It contains the test duration (1-hour or 24-hour), total pad weight gain in grams, and severity classification: mild (<10g/24h), moderate (10-50g/24h), or severe (>50g/24h).

### ValsalvaLeakPointPressure

The ValsalvaLeakPointPressure value object records the abdominal pressure in cmH2O at which urinary leakage occurs. It classifies intrinsic sphincter deficiency (less than 60 cmH2O) versus hypermobility-related stress incontinence (greater than 90 cmH2O), guiding surgical approach selection.

## Male Reproductive Health Value Objects

### SemenParameters

The SemenParameters value object captures WHO-referenced semen analysis results: volume (mL), concentration (million/mL), total count (million), progressive motility (%), total motility (%), and morphology (% normal forms by strict criteria). It classifies abnormalities: oligozoospermia, asthenozoospermia, teratozoospermia, and azoospermia.

### IIEFScore

The IIEFScore value object captures the International Index of Erectile Function questionnaire result. The IIEF-5 (SHIM) abbreviated form produces a score of 5-25, classifying erectile dysfunction severity: severe (5-7), moderate (8-11), mild-moderate (12-16), mild (17-21), or no ED (22-25).

### TestosteroneProfile

The TestosteroneProfile value object contains total testosterone (ng/dL), free testosterone (pg/mL), SHBG (nmol/L), LH (mIU/mL), FSH (mIU/mL), and estradiol (pg/mL). It derives a classification of eugonadal, primary hypogonadism (high LH/FSH with low testosterone), or secondary hypogonadism (low or normal LH/FSH with low testosterone).

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5.
- World Health Organization. WHO Laboratory Manual for the Examination and Processing of Human Semen. 6th Edition. 2021.
