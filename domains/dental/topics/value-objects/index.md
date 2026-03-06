# Value Objects - Dental Domain

## Overview

Value objects are immutable domain objects defined entirely by their attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. In the dental domain, value objects represent measurements, codes, classifications, and clinical descriptors that carry meaning through their values rather than through persistent identity. Probing depth measurements, tooth surface designations, and CDT procedure codes are all naturally modeled as value objects.

## Value Object Design Principles in Dental Systems

Dental value objects encapsulate the precise measurements, standardized codes, and clinical classifications that dental professionals use daily. These objects enforce validity rules at creation time, ensuring that a probing depth cannot be negative, a tooth number must fall within the valid range, and a CDT code must conform to the recognized format. Immutability ensures that once created, these clinical values cannot be accidentally modified.

## Value Objects by Category

### Clinical Measurements

**Probing Depth**
Represents a periodontal probing measurement at a single site. Attributes: depth in millimeters (integer, valid range 1-15), site position (mesiobuccal, buccal, distobuccal, mesiolingual, lingual, distolingual). Two Probing Depth value objects with the same depth and site are equal.

**Clinical Attachment Level**
Represents the distance from the cementoenamel junction to the base of the periodontal pocket. Attributes: level in millimeters (integer), calculated from probing depth and gingival margin position. This value object captures a derived clinical measurement.

**Tooth Mobility**
Represents the degree of tooth mobility on a standardized scale. Attributes: mobility grade (0, 1, 2, or 3) following the Miller classification. Grade 0 indicates physiologic mobility, Grade 3 indicates mobility in all directions including vertical depressibility.

**Bleeding on Probing**
Represents the presence or absence of bleeding at a probing site. Attributes: bleeding status (present or absent) at a specified site. This binary clinical indicator is used in calculating bleeding indices.

### Dental Codes and Identifiers

**CDT Procedure Code**
Represents a Current Dental Terminology procedure code. Attributes: code string (format D followed by four digits), code category, and nomenclature description. The CDT code value object validates format and can determine the procedure category from the code range.

**Tooth Number**
Represents a tooth identifier in the universal numbering system. Attributes: number (1-32 for permanent teeth, A-T for primary teeth). The value object validates the number falls within the valid range and can determine the quadrant, arch, and tooth type from the number.

**Tooth Surface**
Represents one or more surfaces of a tooth involved in a finding or procedure. Attributes: set of surface codes (mesial, occlusal/incisal, distal, buccal/facial, lingual/palatal). The value object enforces that posterior teeth use "occlusal" while anterior teeth use "incisal."

### Clinical Classifications

**Caries Classification**
Represents the severity of a carious lesion. Attributes: classification level (incipient, moderate, advanced, severe) and location type (pit and fissure, smooth surface, root surface). This value object standardizes caries severity assessment across providers.

**Angle Classification**
Represents a malocclusion classification. Attributes: class (I, II Division 1, II Division 2, III) describing the molar relationship. This value object captures the foundational orthodontic assessment.

**Periodontal Disease Classification**
Represents the current periodontal disease status. Attributes: stage (I, II, III, IV) indicating severity and complexity, and grade (A, B, C) indicating rate of progression. This follows the 2017 World Workshop classification system.

### Financial Values

**Fee Amount**
Represents a monetary amount for dental services. Attributes: amount (decimal) and currency code. The value object enforces non-negative amounts and appropriate precision for currency calculations.

**Insurance Benefit**
Represents the insurance coverage determination for a procedure. Attributes: covered amount, patient copay amount, deductible applied, benefit category (preventive, basic, major, orthodontic), and annual maximum remaining. This composite value object captures the financial breakdown of a single procedure.

**Cost Estimate**
Represents the estimated total cost for a treatment plan or individual procedure. Attributes: gross fee, insurance estimated payment, patient estimated responsibility, and estimation date. The value object enforces that estimated payment plus estimated responsibility equals the gross fee.

### Time and Scheduling Values

**Appointment Slot**
Represents an available time block for scheduling. Attributes: date, start time, duration in minutes, operatory identifier, and provider identifier. Two slots with identical attributes are interchangeable.

**Recall Interval**
Represents the recommended time between patient recall visits. Attributes: interval in months (typically 3, 4, or 6), risk level justification (low, moderate, high), and review date. The interval reflects clinical risk assessment.

## Value Object Benefits in the Dental Domain

Value objects provide self-validating types that prevent invalid clinical data from entering the system. A Tooth Number value object that rejects "tooth 33" at creation time is more reliable than a plain integer field validated elsewhere. Immutability ensures that clinical measurements recorded during an examination cannot be inadvertently altered, supporting the clinical record integrity requirements of dental practice.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 5 on value objects.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 6 on value objects.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 5 on value objects and immutability.
