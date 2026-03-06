# Anti-Corruption Layer in the Rheumatology Domain

## Overview

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, terminology, and model structures that do not belong in its domain model. In the rheumatology domain, anti-corruption layers are essential at integration points where rheumatology-specific models interact with external pharmacy systems, laboratory information systems, electronic health records, quality reporting platforms, and regulatory databases. The ACL ensures that each bounded context maintains the purity of its own ubiquitous language and model.

## ACL Between Biologic Therapy and External Pharmacy Systems

The Biologic Therapy Context maintains an internal model of therapy regimens with domain concepts such as BiologicPrescription, InfusionProtocol, and SafetyMonitoring. External pharmacy systems use different terminology and data structures: prescription orders follow NCPDP SCRIPT standards, drug identifiers use NDC codes, and dosing is expressed in pharmacy-specific formats.

The ACL translates between these worlds. When the Biologic Therapy Context sends a prescription, the ACL converts the internal BiologicPrescription into an NCPDP-compliant prescription message. When pharmacy responses arrive (fill status, prior authorization requirements, formulary restrictions), the ACL translates them back into domain events that the Biologic Therapy Context understands (TherapyApproved, PriorAuthorizationRequired, FormularyRestrictionDetected).

Without this ACL, pharmacy-system concepts would leak into the Biologic Therapy Context, forcing clinicians to think in pharmacy terms rather than therapeutic terms.

## ACL Between Clinical Assessment and Laboratory Information Systems

Laboratory information systems (LIS) report serology results using HL7 or FHIR message formats with LOINC codes, reference ranges, and result flags. The Clinical Assessment Context needs these results as SerologyResult value objects with clinical interpretation (positive, negative, equivocal) and clinical significance flags.

The ACL translates LOINC-coded lab results into the domain's SerologyResult value objects. A rheumatoid factor result arrives as a LOINC code (11572-5) with a numeric value and units; the ACL converts this into an RF result with positivity status, titer interpretation, and reference range context. An ANA result with titer and pattern information (homogeneous, speckled, nucleolar) is translated into an ANA value object that the assessment context can use directly.

## ACL Between Outcomes Tracking and Quality Reporting Systems

The Outcomes Tracking Context uses internal models of disease trajectories, functional scores, and patient-reported outcomes. External quality reporting systems (CMS MIPS, HEDIS, PQRS) require data in specific measure formats with particular denominator and numerator definitions.

The ACL translates internal outcome records into quality measure submissions. For example, the MIPS quality measure for disease activity assessment requires documentation that a disease activity score was calculated and a treatment plan was reviewed. The ACL maps the internal OutcomeRecord and DAS28Score into the measure's required data elements without exposing the quality reporting system's structure to the outcomes domain model.

## ACL Between Autoimmune Disease Context and External EHR Systems

When the rheumatology domain integrates with broader electronic health record (EHR) systems, the Autoimmune Disease Context must protect its model from EHR concepts. EHR systems use problem lists with ICD-10 codes, generic medication lists, and progress note templates. The Autoimmune Disease Context uses DiseaseManagementPlan aggregates with classification criteria, disease activity trajectories, and treatment escalation rules.

The ACL translates between ICD-10-coded diagnoses and the domain's richer disease classification model. An ICD-10 code of M05.79 (rheumatoid arthritis with rheumatoid factor, multiple sites) is translated into a fully populated RA classification with ACR/EULAR criteria scores, seropositive status, and disease duration.

## ACL Between Pain Management and External Therapy Provider Systems

The Pain Management Context sends referrals to physical and occupational therapy providers who use their own documentation systems. The ACL translates internal TherapyReferral entities into standardized referral messages and translates incoming progress reports into domain-specific FunctionalProgressReport value objects.

## Design Principles

Anti-corruption layers in the rheumatology domain follow several design principles. The ACL is owned by the downstream context that it protects. It is implemented as a thin translation layer, not a business logic layer. It uses adapter and facade patterns to present external data in domain terms. It fails explicitly when translation is not possible, rather than silently corrupting domain data.

The ACL should not attempt to make external systems conform to internal models. Instead, it accepts external data as-is and translates it into the domain's language. This approach respects the autonomy of external systems while protecting the integrity of the domain model.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9.
- Gamma, Erich, et al. Design Patterns: Elements of Reusable Object-Oriented Software. Addison-Wesley, 1994.
- Benson, Tim, and Grieve, Grahame. Principles of Health Interoperability: FHIR, HL7 and SNOMED CT. Springer, 2021.
