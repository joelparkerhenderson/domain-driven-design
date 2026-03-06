# Energy Standards in Energy

## Overview

Energy standards define the data formats, communication protocols, security requirements, and business practices that govern how energy systems interoperate. These standards serve as published languages that enable integration between bounded contexts, between utilities and ISOs/RTOs, between utilities and customers, and between utilities and regulators. In DDD terms, standards inform value object design, anti-corruption layer translation, and the published languages used at context boundaries.

## Relevance to Energy

The energy industry relies on a complex ecosystem of standards developed by organizations including NERC, IEC, NAESB, IEEE, and the Green Button Alliance. These standards ensure reliable grid operation, secure data exchange, and consistent business practices across thousands of utilities, generators, transmission operators, and market participants. A DDD approach treats each standard as a constraint on the domain model, shaping how value objects are structured, how ACLs translate data, and how integration interfaces are designed.

## Key Energy Standards

### NERC CIP (Critical Infrastructure Protection)

NERC CIP standards define cybersecurity requirements for bulk electric system (BES) assets. They mandate how energy systems must protect control systems, manage access, and respond to security incidents.

- **CIP-002**: BES Cyber System Categorization -- Identify and categorize cyber systems by impact level
- **CIP-003**: Security Management Controls -- Establish cybersecurity policies
- **CIP-004**: Personnel and Training -- Background checks and security awareness training
- **CIP-005**: Electronic Security Perimeters -- Network segmentation and access controls
- **CIP-006**: Physical Security of BES Cyber Systems -- Physical access controls
- **CIP-007**: System Security Management -- Ports, services, patch management, malware prevention
- **CIP-008**: Incident Reporting and Response Planning -- Cybersecurity incident response
- **CIP-009**: Recovery Plans for BES Cyber Systems -- Disaster recovery procedures
- **CIP-010**: Configuration Change Management and Vulnerability Assessments
- **CIP-011**: Information Protection -- Handling of BES Cyber System Information
- **CIP-013**: Supply Chain Risk Management
- **DDD impact**: The Regulatory & Grid Context maintains ComplianceObligation aggregates for each CIP standard. The Generation and T&D contexts implement security controls that the Regulatory context audits.

### IEC 61850

IEC 61850 is the international standard for communication networks and systems in power utility automation, specifically substation automation.

- Defines a common data model for substation equipment (circuit breakers, transformers, protection relays)
- Specifies GOOSE (Generic Object Oriented Substation Event) for fast, peer-to-peer communication
- Specifies MMS (Manufacturing Message Specification) for client-server communication
- Defines SCL (Substation Configuration Language) for describing substation configurations
- Supports sampled values for high-speed measurement data exchange
- **DDD impact**: The Transmission & Distribution Context uses IEC 61850 data models as a basis for substation-related value objects. The ACL between SCADA systems and the domain model translates IEC 61850 objects to internal domain concepts.

### OpenADR (Open Automated Demand Response)

OpenADR is a standard for automated communication of demand response signals between utilities, ISOs, and customer energy management systems.

- Defines event signals (price signals, reliability signals, load control signals)
- Specifies VTN (Virtual Top Node) and VEN (Virtual End Node) communication patterns
- Supports multiple signal types: simple, electricity price, load control, and grid reliability
- Enables automated load curtailment without human intervention
- **DDD impact**: The Regulatory & Grid Context uses OpenADR signal types to model DemandResponseEvent value objects. The integration with customer-side systems follows the VTN/VEN pattern.

### Green Button

Green Button is a standard format for energy usage information, enabling customers to download and share their consumption data.

- Defines ESPI (Energy Services Provider Interface) using Atom feeds and XML
- Specifies interval data formats for consumption, demand, and generation
- Supports Green Button Download (customer-initiated) and Green Button Connect (automated sharing)
- Aligned with NAESB REQ.21 Energy Services Provider Interface
- **DDD impact**: The Metering & Billing Context exports customer data in Green Button format. The MeterReading value object and LoadProfile value object align with Green Button interval data structures.

### CIM (Common Information Model)

CIM (IEC 61968/61970/62325) is a comprehensive data model for the electric power industry, providing a common vocabulary for exchange of information between systems.

- **IEC 61970**: CIM for energy management system (EMS) integration -- generation, transmission, and market operations
- **IEC 61968**: CIM for distribution management system (DMS) integration -- metering, asset management, and work management
- **IEC 62325**: CIM for energy market communications -- market participant data exchange
- Defines hundreds of classes covering every aspect of power system operations
- Used by ISOs/RTOs as the basis for data exchange standards
- **DDD impact**: CIM classes inform the design of entities and value objects across all bounded contexts. The ACL translates between CIM representations and internal domain models.

### NAESB (North American Energy Standards Board)

NAESB standards define business practices for wholesale and retail energy markets in North America.

- **WEQ (Wholesale Electric Quadrant)**: Standards for wholesale market transactions, scheduling, and settlement
- **REQ (Retail Electric Quadrant)**: Standards for retail market operations, customer switching, and data exchange
- Defines electronic data interchange (EDI) transaction sets for energy business processes
- Specifies business practice standards for OASIS (Open Access Same-Time Information System)
- **DDD impact**: The Trading & Market Context and Metering & Billing Context implement NAESB business practices. Contract and settlement value objects align with NAESB-defined data structures.

## Additional Relevant Standards

- **IEEE 1547**: Standard for DER interconnection and interoperability
- **IEEE 2030**: Guide for smart grid interoperability
- **ANSI C12**: Standards for electricity metering (meter accuracy, data formats, communication)
- **IEEE 1366**: Guide for electric power distribution reliability indices (SAIDI, SAIFI, CAIDI)
- **DNP3**: Distributed Network Protocol for SCADA communication

## Relationships to Other Topics

- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate between standard formats and internal models
- [Integration Patterns](../integration-patterns/) -- Standards define many integration interfaces
- [Value Objects](../value-objects/) -- Standards inform value object design and validation rules
- [Regulatory Compliance](../regulatory-compliance/) -- Many standards are regulatory mandates
- [Bounded Contexts](../bounded-contexts/) -- Standards influence context boundary design

## References

- NERC. "Critical Infrastructure Protection (CIP) Standards." North American Electric Reliability Corporation. https://www.nerc.com/pa/Stand/Pages/CIPStandards.aspx
- IEC 61850. "Communication Networks and Systems for Power Utility Automation." International Electrotechnical Commission.
- OpenADR Alliance. "OpenADR 2.0 Specification." https://www.openadr.org/
- Green Button Alliance. "Green Button Data Standard." https://www.greenbuttonalliance.org/
- IEC 61970/61968/62325. "Common Information Model (CIM)." International Electrotechnical Commission.
- NAESB. "Wholesale and Retail Electric Standards." North American Energy Standards Board. https://www.naesb.org/
