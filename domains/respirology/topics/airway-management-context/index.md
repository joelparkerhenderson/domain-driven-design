# Airway Management Context

## Overview

The Airway Management Context handles all procedures related to establishing, maintaining, and protecting the patient's airway. It covers intubation, tracheostomy, airway clearance techniques, and bronchodilator therapy administration. This bounded context is classified as a supporting subdomain because it follows well-established procedural protocols, though the accurate documentation and protocol adherence it enforces are essential for patient safety.

This context interacts closely with the Ventilatory Support Context through a shared kernel of airway device concepts, and it publishes procedure completion events consumed by the Chronic Disease Management Context for longitudinal tracking.

## Ubiquitous Language

Key terms within this context include: intubation, extubation, endotracheal tube, tracheostomy, tracheostomy tube, decannulation, airway clearance, chest physiotherapy, postural drainage, percussion, vibration, oscillatory positive expiratory pressure (PEP), cough augmentation, mechanical insufflation-exsufflation, suctioning, bronchodilator, short-acting beta-agonist (SABA), long-acting beta-agonist (LABA), short-acting muscarinic antagonist (SAMA), long-acting muscarinic antagonist (LAMA), metered-dose inhaler (MDI), dry powder inhaler (DPI), nebulizer, spacer, and inhaler technique assessment.

## Aggregate: Airway Procedure

The Airway Procedure is the aggregate root, representing a single airway management intervention.

**Components**:
- Procedure ID (unique identifier)
- Patient ID (reference)
- Procedure type (intubation, extubation, tracheostomy, tracheostomy change, decannulation)
- Performing clinician ID
- Procedure date and time
- Technique details (direct laryngoscopy, video laryngoscopy, surgical, percutaneous)
- Device specification (tube type, size, cuff type, manufacturer)
- Number of attempts (for intubation)
- Confirmation method (capnography, chest X-ray, clinical assessment)
- Complications (none, difficult airway, failed attempt, trauma, desaturation)
- Pre-procedure assessment (airway assessment score, NPO status, medication)
- Procedure status (planned, in-progress, completed, cancelled)

**Invariants**:
- An intubation procedure must record the confirmation method used to verify tube placement.
- A tracheostomy procedure must document the tube type and size.
- A completed procedure must record the performing clinician and the outcome.
- The number of intubation attempts must be recorded and flagged if exceeding institutional thresholds.

## Entities

**Intubation Record**: Tracks the details of an intubation event including airway difficulty assessment (Mallampati score, thyromental distance), laryngoscope type, blade size, tube size and depth at teeth, and confirmation results. This entity persists as a permanent record referenced by the Ventilatory Support Context.

**Tracheostomy Record**: Tracks the tracheostomy from insertion through ongoing care to decannulation. Includes the surgical approach, tube specifications, initial care protocols, cuff management details, tube change schedule, and decannulation readiness assessments.

**Airway Clearance Session**: Records a single airway clearance treatment including the technique applied, duration, patient positioning, sputum production (volume, color, consistency), patient tolerance, and pre/post vital signs.

**Bronchodilator Administration Record**: Documents the administration of bronchodilator therapy including the medication, dose, delivery device, technique assessment, pre- and post-treatment peak flow or spirometry values, and patient response.

## Value Objects

**TubeSpecification**: Describes the airway device with tube type (endotracheal, tracheostomy, laryngeal mask), internal diameter (mm), length, cuff type (cuffed, uncuffed, foam cuff), and material.

**BronchodilatorDose**: Captures the drug name, drug class (SABA, LABA, SAMA, LAMA, combination), dose amount and unit, and delivery method (MDI with spacer, DPI, nebulizer, IV).

**AirwayClearanceParameters**: Specifies the technique type, duration, frequency, patient position, and any adjunctive devices used.

**AirwayAssessmentScore**: Captures pre-intubation airway assessment components including Mallampati class (I-IV), mouth opening, neck mobility, and overall difficulty prediction.

**SputumCharacteristics**: Describes sputum volume (scant, moderate, copious), color (clear, white, yellow, green, brown, bloody), consistency (thin, thick, tenacious), and odor.

## Domain Events

**AirwayEstablished**: Published when an airway is successfully secured. Carries the procedure ID, patient ID, device specifications, and confirmation details. Consumed by the Ventilatory Support Context to initiate ventilator setup.

**ExtubationCompleted**: Published when an endotracheal tube is removed. Carries the patient ID, post-extubation respiratory status, and any complications.

**AirwayClearanceCompleted**: Published when a clearance session finishes. Consumed by the Chronic Disease Management Context for tracking sputum trends and treatment adherence.

**BronchodilatorAdministered**: Published when bronchodilator therapy is delivered. Carries pre- and post-treatment measurements. Consumed by the Chronic Disease Management Context.

**DecannulationCompleted**: Published when a tracheostomy tube is permanently removed. Consumed by the Ventilatory Support Context and Pulmonary Rehabilitation Context.

## Domain Services

**AirwayClearanceProtocolService**: Selects the appropriate clearance technique and parameters based on the patient's diagnosis, secretion characteristics, mobility, and clinical status.

**BronchodilatorResponseAssessmentService**: Evaluates pre- and post-bronchodilator measurements to determine whether the response meets criteria for clinically significant reversibility (per ATS/ERS criteria: FEV1 increase of 200 mL and 12% from baseline).

**InhalerTechniqueAssessmentService**: Evaluates a patient's inhaler technique against standardized checklists for each device type and identifies specific errors requiring correction.

## Integration Points

- **Shared Kernel with Ventilatory Support Context**: Common types for airway devices, tube specifications, and ventilation interfaces.
- **Downstream**: Publishes procedure events consumed by Ventilatory Support, Chronic Disease Management, and Pulmonary Rehabilitation contexts.
- **External**: Interfaces with pharmacy systems for medication data through an anti-corruption layer.

## References

- Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.
- Vernon, V. (2013). *Implementing Domain-Driven Design*. Addison-Wesley.
- Khononov, V. (2021). *Learning Domain-Driven Design*. O'Reilly Media.
- Higgs, A., et al. (2018). *Guidelines for the Management of Tracheal Intubation in Critically Ill Adults*. British Journal of Anaesthesia.
- Hill, A. T., et al. (2019). *British Thoracic Society Guideline for Bronchiectasis in Adults*. Thorax.
