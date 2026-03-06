# Device Management Context

## Overview

The Device Management Context covers the selection, fitting, programming, verification, and maintenance of hearing devices. This bounded context manages the lifecycle of hearing aids, cochlear implants, bone-anchored hearing systems, and assistive listening devices. It translates diagnostic findings from the Hearing Assessment Context into actionable device configurations that improve the patient's hearing function.

## Strategic Importance

As a supporting subdomain, the Device Management Context implements clinical recommendations produced by the core Hearing Assessment Context. Device fitting follows well-established prescriptive formulae and manufacturer protocols. The domain model must accurately represent device capabilities, fitting parameters, and verification procedures while maintaining independence from any single manufacturer's proprietary systems.

## Ubiquitous Language

Key terms within this bounded context include: hearing aid, cochlear implant, bone-anchored hearing system, assistive listening device, fitting, programming, gain, compression, output, channel, program, real ear measurement (REM), fitting prescription, prescriptive target, NAL-NL2, DSL v5, electroacoustic analysis, feedback management, directional microphone, noise reduction, frequency lowering, telecoil, Bluetooth connectivity, trial period, warranty, and maintenance.

## Aggregates

### Device Fitting

The Device Fitting aggregate is the central unit of this context. Rooted by the DeviceFitting entity, it contains a FittingPrescription value object (target amplification derived from audiometric data), DeviceConfiguration value objects (current programming parameters for each program), and RealEarMeasurement value objects (probe microphone verification results). The aggregate enforces safety limits: maximum power output must not exceed uncomfortable loudness levels, and gain settings must not create feedback or exceed the device's physical capability.

### Device

The Device aggregate tracks the physical hearing device through its lifecycle. Rooted by the Device entity (identified by serial number), it contains DeviceSpecification value objects (manufacturer, model, style, technology level), MaintenanceRecord value objects (repair history, battery replacement, cleaning), and WarrantyStatus. The aggregate tracks the device from acquisition through daily use to eventual replacement or retirement.

## Value Objects

FittingPrescription contains target gain at each frequency, compression ratios, and maximum power output limits, along with the prescriptive formula used. RealEarMeasurement contains measured output at each frequency and the deviation from target. DeviceSpecification describes the device's technical characteristics. DeviceConfiguration captures the current programming parameters for a specific program. MaintenanceRecord documents a maintenance event with date, type, and outcome.

## Domain Events

DeviceFittingCompleted is published when a fitting session is finalized, including verification results. This event triggers clinical report generation and informs the Rehabilitation Context about the patient's current amplification. DeviceVerificationFailed is published when real ear measurements reveal unacceptable deviation from prescriptive targets. DeviceMaintenanceRequired is published when scheduled maintenance is due or a problem is reported. DeviceReturned is published when a patient returns a device during the trial period.

## Domain Services

The PrescriptiveTargetCalculationService computes fitting targets from audiometric data using selected formulae (NAL-NL2 for adults, DSL v5 for pediatric fittings). The FittingVerificationService compares real ear measurements against prescriptive targets and determines whether the fit is acceptable. The DeviceCandidacyService evaluates whether a patient is a candidate for specific device types based on audiometric, medical, and lifestyle factors.

## Business Rules

Several business rules govern this context. Maximum power output must never exceed the patient's measured or estimated uncomfortable loudness level, protecting against acoustic trauma. Fitting prescriptions must be recalculated whenever the patient's audiometric data is updated. Real ear measurement verification is required for all new fittings per best practice guidelines. Pediatric fittings must use the DSL v5 prescriptive formula with age-appropriate corrections. Device warranty tracking must account for manufacturer-specific terms and conditions.

## Workflow

The typical workflow begins when the Device Management Context receives an EvaluationCompleted event from the Hearing Assessment Context. The DeviceCandidacyService evaluates the patient's suitability for amplification. If candidacy is confirmed, the audiologist selects a device based on the patient's hearing loss, lifestyle, cosmetic preferences, and budget. The PrescriptiveTargetCalculationService generates fitting targets. The device is programmed, verified through real ear measurement, and the fitting is finalized. The patient begins a trial period during which adjustments may be made.

## External Interfaces

This context maintains anti-corruption layers for hearing device manufacturer programming interfaces. Manufacturers such as Phonak, Oticon, ReSound, Signia, Starkey, and Widex each provide proprietary fitting software with different data models and APIs. The ACL translates between manufacturer-specific representations and the domain model's standardized fitting concepts. This context also interfaces with the NOAH platform, an industry-standard middleware for audiometric and fitting data exchange.

## Manufacturer Independence

A key design goal is manufacturer independence. The domain model represents fitting concepts in manufacturer-neutral terms. A FittingPrescription specifies target gain at standard audiometric frequencies regardless of the device manufacturer. The DeviceConfiguration value object captures programming parameters in standardized terms that the ACL translates to and from manufacturer-specific formats. This design allows the practice to work with any manufacturer without changing the domain model.

## References

- Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley.
- Vernon, V. (2013). Implementing Domain-Driven Design. Addison-Wesley.
- Khononov, V. (2021). Learning Domain-Driven Design. O'Reilly Media.
- Dillon, H. (2012). Hearing Aids. 2nd Edition. Thieme.
- American Academy of Audiology (AAA). Clinical Practice Guidelines: Adult Amplification.
- Hearing Instrument Manufacturers' Software Association (HIMSA). NOAH System Documentation.
