# Diagnostic Imaging Context

## Overview

The Diagnostic Imaging Context is a supporting bounded context in the ophthalmology
domain that manages the acquisition, storage, interpretation, and reporting of
ophthalmic diagnostic imaging and functional testing. This context owns the models
for optical coherence tomography (OCT), fundus photography, fluorescein angiography,
and visual field perimetry. It serves as a critical data supplier to multiple downstream
contexts including Glaucoma Management, Retinal Care, and Surgical Management.

## Scope

This context covers:

- Optical coherence tomography (OCT) for retinal and optic nerve imaging, including
  macular scans, RNFL analysis, and ganglion cell analysis.
- Fundus photography for documentation of retinal pathology, optic nerve appearance,
  and anterior segment conditions.
- Fluorescein angiography (FA) and indocyanine green angiography (ICG) for vascular
  imaging of the retina and choroid.
- Visual field testing (perimetry) using standard automated perimetry (SAP) and
  other perimetric methods for functional assessment of vision.
- Corneal topography and tomography for corneal surface and thickness mapping.
- Ocular biometry for IOL power calculation measurements.
- Image quality assessment and study lifecycle management.

## Aggregates

**ImagingStudy** is the root aggregate representing a complete diagnostic imaging
investigation. It contains one or more imaging series, tracks the study lifecycle
from order through acquisition to interpretation, and maintains the association
between technical data and clinical interpretation.

**OCTScan** is a specialized aggregate for optical coherence tomography. It contains
scan protocol parameters, raw scan data references, automated layer segmentation
results, thickness measurements, and normative comparisons. The aggregate enforces
consistency between scan parameters and the selected protocol.

**VisualFieldTest** is an aggregate capturing automated perimetric data. It contains
stimulus parameters, patient response data, reliability indices (fixation losses,
false positives, false negatives), and derived statistical metrics (mean deviation,
pattern standard deviation, visual field index).

## Key Entities

- ImagingOrder: the request for an imaging study with clinical indication.
- ImageSeries: a set of images acquired with consistent parameters.
- ImagingInterpretation: the clinician's reading of imaging data.

## Key Value Objects

- ScanProtocol: technical parameters defining the imaging acquisition.
- RetinalThickness: OCT-derived thickness measurement at a specific location.
- MeanDeviation: summary statistic from visual field testing.
- PatternStandardDeviation: localized defect measure from visual field testing.
- ReliabilityIndex: quality metric for patient test compliance.

## Domain Events Published

- ImagingStudyOrdered: signals a new imaging request.
- ImagingStudyAcquired: signals completion of image acquisition.
- ImagingStudyInterpreted: signals finalization of clinical interpretation.
  This event is consumed by Glaucoma Management (OCT RNFL and visual field results),
  Retinal Care (OCT macular and angiography results), and Surgical Management
  (biometry and topography results).

## Domain Events Consumed

- ExaminationCompleted (from Clinical Examination): correlates imaging results
  with clinical findings from the same encounter.
- SurgicalCaseScheduled (from Surgical Management): triggers acquisition of
  pre-operative imaging studies.

## Domain Services

- ImagingCorrelationService: correlates findings across multiple imaging modalities
  and time points for the same patient.
- ImageQualityAssessmentService: evaluates technical quality of acquired imaging
  data against established criteria.

## Business Rules

- Every imaging study must have a valid clinical indication and ordering clinician.
- OCT scans with signal strength below the manufacturer's threshold must be flagged
  for potential repeat acquisition.
- Visual field tests with reliability indices exceeding thresholds (fixation losses
  greater than 20%, false positives greater than 15%) must be flagged as unreliable.
- Imaging interpretations must be finalized by a qualified clinician (ophthalmologist
  or supervised resident).
- Studies transition through ordered, scheduled, acquired, interpreted, and finalized
  states in sequence; no state may be skipped.

## Integration Points

- Downstream from Clinical Examination: receives imaging orders based on clinical
  examination findings.
- Upstream to Glaucoma Management: provides OCT RNFL and visual field data.
- Upstream to Retinal Care: provides OCT macular, fundus, and angiography data.
- Upstream to Surgical Management: provides biometry and topography data.
- External PACS integration: uses DICOM standards through open host service.
- External EHR integration: transmits results through anti-corruption layer.

## DICOM Compliance

The Diagnostic Imaging Context uses DICOM (Digital Imaging and Communications in
Medicine) as its published language for imaging data exchange. DICOM defines the
standard for image formats, metadata, and communication protocols. The context's
open host service exposes DICOM-compliant interfaces for integration with PACS and
other imaging systems.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly Media, 2021.
- DICOM Standards Committee. *DICOM Standard*. https://www.dicomstandard.org/
- American Academy of Ophthalmology. *Ophthalmic Imaging* BCSC Section 3.
