# Anti-Corruption Layer in Gastroenterology

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context
from the concepts, terminology, and model structures of external systems. In the
gastroenterology domain, ACLs prevent model pollution from electronic health record
systems, laboratory information systems, pharmacy platforms, radiology systems, and
other external integrations.

## Purpose

Gastroenterology systems must integrate with numerous external platforms that each have
their own data models. A laboratory information system represents lab results differently
than the gastroenterology domain model requires. A pharmacy system models medications
using its own classification hierarchy. Without anti-corruption layers, these external
representations would leak into the domain model, compromising its integrity and
making it increasingly difficult to reason about.

## ACL for Electronic Health Record Integration

The EHR system maintains a comprehensive but generalist patient model. It represents
clinical encounters, diagnoses, and orders using broad healthcare schemas such as
HL7 FHIR resources. The gastroenterology domain requires more specific representations.

The ACL translates EHR encounter records into the Clinical Assessment Context's
PatientEncounter aggregate, extracting only gastroenterology-relevant information
and mapping generic diagnosis codes to the domain's specific diagnostic categories.
An EHR "Condition" resource becomes a domain-specific ClinicalFinding with
gastroenterology-appropriate classification.

## ACL for Laboratory Information Systems

Lab information systems report results using LOINC codes, reference ranges, and
standardized units. The gastroenterology domain model needs these results interpreted
within clinical context. The ACL translates raw lab results into domain value objects.

For example, a hepatology-relevant lab panel (AST, ALT, bilirubin, albumin, INR)
arrives as individual result records. The ACL composes these into a LiverFunctionPanel
value object that the Hepatology Context can use directly for Child-Pugh and MELD
score calculation. The translation encapsulates the mapping logic and protects the
domain from changes in how the lab system reports results.

## ACL for Pharmacy Systems

Pharmacy systems model medications by NDC code, dosage form, and dispensing instructions.
The Inflammatory Disease Context models biologic therapies by mechanism of action,
indication, dosing protocol, and monitoring requirements. The ACL translates between
pharmacy medication records and the domain's TherapyRegimen entities, mapping NDC
codes to the domain's biologic therapy classification.

## ACL for Radiology and Imaging Systems

Radiology systems provide imaging results through DICOM standards and structured
reports. The Clinical Assessment Context needs imaging findings translated into
its own diagnostic framework. The ACL extracts relevant findings from radiology
reports and creates domain-specific ImagingResult value objects, filtering out
non-gastroenterology findings and translating radiological terminology into the
domain's ubiquitous language.

## ACL for Pathology Systems

Pathology results from endoscopic biopsies arrive in pathology system formats with
synoptic reporting structures. The ACL translates these into PathologyResult value
objects consumed by the Endoscopic Procedures Context and the Inflammatory Disease
Context. The translation maps pathology classification schemes to the domain's
histological grading models.

## Implementation Principles

Anti-corruption layers in the gastroenterology domain follow several principles:

1. Translation is unidirectional: the ACL translates external models into domain
   models, never exposing domain internals to external systems.
2. Each bounded context maintains its own ACLs for the external systems it integrates
   with, even if multiple contexts integrate with the same external system.
3. ACL logic is isolated in dedicated translation components, separate from domain
   logic and infrastructure concerns.
4. When external system APIs change, only the ACL implementation changes; the domain
   model remains stable.

## Design Considerations

The granularity of ACL translation depends on how different the external model is
from the domain model. Laboratory results require relatively straightforward mapping.
EHR encounter records require more substantial transformation. Pharmacy integration
may require lookup tables to map between classification systems.

## References

- Evans, E. (2003). *Domain-Driven Design*. Addison-Wesley. Chapter 14: Anti-Corruption Layer.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley. Chapter 3: Context Mapping.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media. Chapter 7: Anti-Corruption Layer Pattern.
- Hohpe, G. & Woolf, B. (2003). *Enterprise Integration Patterns*. Addison-Wesley.
