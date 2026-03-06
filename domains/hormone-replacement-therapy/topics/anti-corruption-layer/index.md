# Anti-Corruption Layer in the Hormone Replacement Therapy Domain

An anti-corruption layer (ACL) is a translation boundary that protects a bounded context from external system concepts, data formats, and modeling assumptions that do not align with its internal domain model. In the hormone replacement therapy (HRT) domain, ACLs are essential at integration points between bounded contexts and between the domain and external clinical, pharmacy, and regulatory systems.

## Purpose

HRT systems must integrate with diverse external systems including laboratory information systems, pharmacy management platforms, electronic health records, and regulatory reporting services. Each external system imposes its own data model, terminology, and assumptions. Without ACLs, these external concepts leak into the domain model, corrupting the ubiquitous language and creating fragile coupling. ACLs translate between external representations and internal domain concepts, preserving model integrity.

## Internal Anti-Corruption Layers

### Treatment Optimization ACL against Lab Monitoring

The Treatment Optimization Context maintains an ACL when consuming data from the Lab Monitoring Context. Lab Monitoring represents results as LabResult value objects with reference ranges, units, and collection timestamps. Treatment Optimization needs these results reframed as TitrationInput objects that include trend direction, distance from therapeutic target, and rate of change. The ACL performs this translation, ensuring that optimization algorithms operate on purpose-built representations rather than raw lab data.

### Side Effect Management ACL against Clinical Assessment

The Side Effect Management Context maintains an ACL when receiving risk factor updates from the Clinical Assessment Context. Clinical Assessment represents risk factors as part of a comprehensive patient profile with clinical terminology. Side Effect Management needs risk factors translated into its own RiskWeight value objects that quantify the contribution of each factor to specific adverse event probabilities. The ACL transforms clinical descriptions into pharmacovigilance-oriented risk quantifications.

### Hormone Protocol ACL against Treatment Optimization

The Hormone Protocol Context maintains an ACL when receiving optimization recommendations from the Treatment Optimization Context. Optimization produces recommendations in analytical terms such as percentage dose adjustments and relative formulation rankings. The Protocol Context needs these translated into concrete prescribing actions such as specific formulation names, exact dosages, and administration instructions. The ACL bridges analytical recommendations and clinical prescribing.

## External Anti-Corruption Layers

### Laboratory Information System Integration

The Lab Monitoring Context integrates with external laboratory information systems (LIS) that communicate using HL7 v2 ORU messages or FHIR DiagnosticReport resources. The ACL translates these standard healthcare interoperability formats into domain-specific LabResult value objects. It maps external LOINC codes to internal test identifiers, normalizes units of measurement, handles result qualifiers, and resolves discrepancies between external reference ranges and domain-defined therapeutic target ranges.

### Pharmacy System Integration

The Hormone Protocol Context integrates with external pharmacy systems for prescription transmission and fulfillment status tracking. The ACL translates between the domain's TreatmentProtocol aggregate and pharmacy-specific formats such as NCPDP SCRIPT messages or FHIR MedicationRequest resources. It maps internal formulation identifiers to NDC codes, translates dosing schedules into SIG codes, and converts protocol status into pharmacy workflow states.

### Electronic Health Record Integration

Multiple contexts integrate with electronic health record (EHR) systems. Each maintains its own ACL tailored to its specific data needs. The Clinical Assessment Context translates EHR patient demographics and medical history into its assessment model. The Lab Monitoring Context translates EHR lab orders into its monitoring plan structure. The ACLs prevent EHR data models, which are designed for broad clinical documentation, from imposing their generalized structure on the specialized HRT domain models.

### Regulatory Reporting Integration

The Side Effect Management Context integrates with regulatory reporting systems such as FDA MedWatch for adverse event reporting and state pharmacy board systems for controlled substance monitoring. The ACL translates internal AdverseEventRecord aggregates into the specific reporting formats required by each regulatory body, including CIOMS forms for international reporting and MedWatch 3500A forms for FDA reporting. The ACL also translates regulatory feedback into internal domain events.

## Implementation Considerations

ACLs in the HRT domain are implemented as dedicated translation services that sit at context boundaries. They are stateless, converting representations without maintaining their own persistent state. ACLs contain mapping logic, format conversion, and validation to ensure that incoming data meets the receiving context's invariants. When translation is not possible due to missing or invalid data, the ACL raises integration exceptions rather than passing corrupt data into the domain model.

## Testing Strategy

ACLs require thorough testing because they are the primary defense against model corruption. Test cases cover correct translation of well-formed inputs, handling of missing or null fields, unit conversion accuracy, code mapping completeness, and graceful degradation when external systems send unexpected formats. Contract tests verify that ACL expectations align with actual external system outputs.

## References

- Evans, Eric. Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley, 2003. Chapter 14 on anti-corruption layers.
- Vernon, Vaughn. Implementing Domain-Driven Design. Addison-Wesley, 2013. Chapter 3 on context mapping and integration.
- Khononov, Vlad. Learning Domain-Driven Design. O'Reilly Media, 2021. Chapter 9 on communication patterns.
- Benson, Tim and Grieve, Grahame. Principles of Health Interoperability: FHIR, HL7 and SNOMED CT. Springer, 2021.
