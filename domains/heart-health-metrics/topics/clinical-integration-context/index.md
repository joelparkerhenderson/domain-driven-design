# Clinical Integration Context

The Clinical Integration Context is the bounded context responsible for interoperability with external clinical systems. It handles HL7 FHIR data exchange, electronic health record (EHR) synchronization, clinical decision support (CDS) hook integration, and translation between the heart health metrics domain model and standardized clinical data representations.

## Overview

The Clinical Integration Context bridges the heart health metrics domain and the broader healthcare information ecosystem. It ensures that cardiac monitoring data, analysis findings, risk assessments, and alerts can flow into and out of hospital EHR systems, health information exchanges, and clinical decision support platforms using standardized protocols and data formats.

This context is classified as a generic subdomain because it primarily implements established interoperability standards (HL7 FHIR, CDS Hooks) rather than novel clinical logic. However, correct implementation is essential for clinical workflow integration, regulatory compliance, and the practical utility of the cardiac monitoring system within healthcare delivery organizations.

## Key Concepts

**FHIR Resource Mapping.** The context maps domain objects to HL7 FHIR resources. Heart Rate Readings map to FHIR Observation resources with LOINC code 8867-4. Blood Pressure Readings map to FHIR Observation with component elements for systolic (LOINC 8480-6) and diastolic (LOINC 8462-4). ECG recordings map to FHIR DiagnosticReport with associated Media resources. Risk assessments map to FHIR RiskAssessment resources.

**EHR Synchronization.** The context manages bidirectional data flow with electronic health record systems. Outbound synchronization pushes cardiac monitoring results, risk scores, and alerts to the EHR. Inbound synchronization pulls patient demographics, medication lists, laboratory results, and clinical history needed for risk assessment and contextual analysis.

**CDS Hooks Integration.** The context exposes clinical decision support hooks that EHR systems can invoke during clinical workflows. When a clinician opens a patient's chart, a CDS hook may return cardiac monitoring findings, risk alerts, or recommended actions based on the latest cardiac health data.

**SMART on FHIR Applications.** The context supports launching SMART on FHIR applications from within the EHR, providing clinicians with rich cardiac health dashboards and detailed analysis views while maintaining the EHR's authentication and authorization context.

**Terminology Mapping.** The context translates between the domain's ubiquitous language and standardized clinical terminologies. Cardiac conditions are mapped to SNOMED CT codes. Measurements are mapped to LOINC codes. Medications are mapped to RxNorm codes. This mapping ensures that cardiac health data is universally interpretable across healthcare systems.

**Anti-Corruption Layer.** The context employs anti-corruption layers to prevent external clinical system models from corrupting the heart health metrics domain model. FHIR resource structures and EHR data models are translated at the boundary rather than adopted internally.

## Domain Examples

A remote cardiac monitoring service integrates with a hospital's Epic EHR. The Clinical Integration Context maps the patient's latest 7-day blood pressure trend (a BP Trend Analysis value object from the Cardiac Analysis Context) to a FHIR DiagnosticReport resource containing the individual Observation resources for each reading, the computed trend summary, and the ACC/AHA staging classification. The report is posted to the EHR via the FHIR REST API and appears in the cardiologist's inbox.

During a clinic visit, the cardiologist opens the patient's chart in the EHR. A CDS Hook fires, invoking the Clinical Integration Context, which returns a card displaying the patient's current ASCVD risk score of 14.2% (intermediate risk), a 3-month blood pressure trend showing improvement, and a recommendation to continue current antihypertensive therapy. The cardiologist can launch a SMART on FHIR app for a detailed cardiac health dashboard.

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/index.md) - EHR integration ACLs protect the domain model.
- [Reporting & Visualization Context](../reporting-visualization-context/index.md) - Reports may be exported through clinical integration.
- [Heart Health Metrics Standards](../heart-health-metrics-standards/index.md) - FHIR, LOINC, SNOMED CT standards.
- [Integration Patterns](../integration-patterns/index.md) - Published language and open host service patterns.
- [Regulatory Compliance](../regulatory-compliance/index.md) - HIPAA and interoperability mandates.
- [Subdomains](../subdomains/index.md) - Classified as a generic subdomain.

## References

- Evans, Eric. *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley, 2003.
- Vernon, Vaughn. *Implementing Domain-Driven Design*. Addison-Wesley, 2013.
- Khononov, Vlad. *Learning Domain-Driven Design*. O'Reilly, 2021.
- HL7 FHIR Specification, R4. https://www.hl7.org/fhir/
- Mandel, J.C. et al. "SMART on FHIR: A Standards-Based, Interoperable Apps Platform for Electronic Health Records." *JAMIA*, 23(5), 899-908, 2016.
- Benson, Tim. *Principles of Health Interoperability: HL7 and SNOMED*. Springer, 2012.
- CDS Hooks Specification. https://cds-hooks.org/
