# Regulatory Compliance in Logistics

## Overview

Regulatory compliance in logistics encompasses the legal and governance requirements that shape how freight is transported, drivers are managed, vehicles are maintained, goods cross borders, and hazardous materials are handled. In Domain-Driven Design, regulatory compliance is a cross-cutting concern that influences domain model design across multiple bounded contexts -- from driver HOS rules in Fleet Management to customs filing requirements in the Customs & Clearance Context.

## Relevance to Logistics

Logistics is one of the most heavily regulated industries. A single compliance violation can result in vehicle impoundment, driver disqualification, shipment seizure, civil penalties of $10,000-$100,000+, or loss of operating authority. Regulatory requirements are not optional business rules -- they are legally mandated constraints that the domain model must enforce. The domain model must make compliance violations structurally impossible where feasible and detectable where prevention is not possible.

## FMCSA and DOT Regulations

### Federal Motor Carrier Safety Administration (FMCSA)

FMCSA regulates interstate commercial motor vehicle operations in the United States.

- **Operating authority (MC number)**: Carriers must hold active operating authority to transport freight for hire. The domain model validates carrier authority status before tender acceptance.
- **Safety fitness determination**: FMCSA assigns safety ratings (Satisfactory, Conditional, Unsatisfactory) based on compliance reviews. Carriers with Unsatisfactory ratings are prohibited from operating.
- **CSA (Compliance, Safety, Accountability)**: FMCSA's data-driven safety program that tracks carrier performance across seven BASICs (Behavioral Analysis and Safety Improvement Categories). The Carrier entity in Freight Brokerage Context should reference CSA scores.
- **Insurance requirements**: Carriers must maintain minimum insurance coverage ($750,000-$5,000,000 depending on cargo type). Insurance expiration triggers carrier suspension in the domain model.

### DOT Vehicle Regulations

- **Annual inspections**: Commercial vehicles must pass annual DOT inspections per 49 CFR Part 396. The Vehicle aggregate tracks inspection dates and results.
- **Pre-trip and post-trip inspections**: Drivers must conduct daily vehicle inspections per 49 CFR 396.13. Driver Vehicle Inspection Reports (DVIRs) are entities within the Vehicle aggregate.
- **Vehicle markings**: Commercial vehicles must display DOT number, legal name, and MC number per 49 CFR Part 390.21.

## Hours of Service (HOS) Regulations

### 49 CFR Part 395

HOS rules limit driving and on-duty time for commercial motor vehicle drivers to prevent fatigue-related accidents.

- **11-hour driving limit**: A driver may drive a maximum of 11 hours after 10 consecutive hours off duty
- **14-hour on-duty limit**: A driver may not drive beyond the 14th consecutive hour after coming on duty, regardless of breaks taken
- **30-minute break requirement**: A driver must take a 30-minute break after 8 cumulative hours of driving
- **60/70-hour limit**: A driver may not drive after 60 hours on duty in 7 consecutive days or 70 hours in 8 consecutive days
- **34-hour restart**: A driver may restart the 60/70-hour clock after taking 34 or more consecutive hours off duty
- **Sleeper berth provision**: A driver using a sleeper berth may split the 10-hour off-duty period into two periods (7/3 or 8/2 split)
- **Domain modeling**: HOSWindow value object calculates remaining driving time, on-duty time, and mandatory break timing. The DispatchService validates HOS availability before authorizing departure.

### ELD Mandate (49 CFR Part 395 Subpart B)

- All commercial motor vehicles subject to HOS rules must use a certified ELD
- ELDs automatically record driving time based on vehicle engine data
- Drivers and carriers must retain ELD records for 6 months
- Fleet Management Context integrates with ELD providers via anti-corruption layer adapters

## HAZMAT Regulations (49 CFR Parts 171-180)

### Hazardous Materials Transportation

- **Classification**: Hazardous materials are classified into 9 hazard classes (explosives, gases, flammable liquids, etc.) with specific packaging, labeling, and placarding requirements
- **Shipping papers**: HAZMAT shipments require shipping papers with proper shipping name, hazard class, UN identification number, packing group, and emergency contact
- **Driver endorsement**: Drivers transporting HAZMAT must hold a Hazmat endorsement (H) on their CDL, requiring TSA background check
- **Routing restrictions**: Certain hazmat classes are prohibited on specific routes (tunnels, bridges, populated areas). Route Optimization Context must enforce hazmat routing constraints.
- **Domain modeling**: HazmatClassification value object with hazard class, UN number, and packing group. Route constraints include hazmat-restricted road segments.

## Customs and Trade Regulations

### US Customs and Border Protection (CBP)

- **Entry filing**: Importers must file customs entries (Entry Summary, CBP Form 7501) within 15 calendar days of arrival
- **Duty payment**: Duties, taxes, and fees must be paid within 10 working days of entry
- **Reasonable care standard**: Importers must exercise reasonable care in classifying goods, determining value, and claiming trade preferences
- **Recordkeeping**: Import records must be retained for 5 years per 19 CFR Part 163

### C-TPAT (Customs-Trade Partnership Against Terrorism)

- **Purpose**: Voluntary US government-business partnership to strengthen international supply chain security
- **Benefits**: Reduced CBP examinations, priority processing, front-of-line inspections
- **Requirements**: Supply chain security profile, self-assessment, CBP validation
- **Domain modeling**: TradePartner entity includes C-TPAT certification status and tier level

### Export Controls

- **EAR (Export Administration Regulations)**: Controls export of dual-use items based on ECCN (Export Control Classification Number)
- **ITAR (International Traffic in Arms Regulations)**: Controls export of defense articles and services
- **OFAC Sanctions**: Treasury Department sanctions prohibiting transactions with certain countries, entities, and individuals
- **Domain modeling**: TradeComplianceService screens shipments against denied party lists and export control classifications before filing

## GDPR and Data Privacy

### General Data Protection Regulation (EU)

- **Applicability**: Logistics companies operating in or serving EU residents must comply with GDPR for driver personal data, customer delivery addresses, and GPS tracking data
- **Driver data**: GPS tracking, ELD records, and performance data are personal data under GDPR
- **Customer data**: Delivery addresses, contact information, and delivery preferences require consent and purpose limitation
- **Data retention**: Personal data must not be retained beyond the period necessary for its purpose
- **Domain modeling**: Data retention policies are enforced at the repository level; personal data fields are flagged for GDPR handling

## Cabotage Laws

### Domestic Transportation Restrictions

- **Definition**: Cabotage laws restrict the right to transport goods within a country to domestically registered carriers
- **US cabotage**: The Jones Act restricts domestic waterborne commerce to US-flagged vessels. Domestic trucking requires US operating authority.
- **EU cabotage**: Non-resident EU carriers may perform limited cabotage operations (3 operations within 7 days after international delivery) per Regulation (EC) 1072/2009
- **Canadian cabotage**: Foreign carriers may only perform limited cabotage under specific conditions
- **Domain modeling**: The CarrierMatchingService validates carrier eligibility for domestic movements based on operating authority and cabotage restrictions

## Relationships to Other Topics

- [Fleet Management Context](../fleet-management-context/) -- FMCSA, DOT, HOS, and ELD regulations shape vehicle and driver management
- [Route Optimization Context](../route-optimization-context/) -- HAZMAT routing and HOS constraints affect route planning
- [Freight Brokerage Context](../freight-brokerage-context/) -- Carrier authority, insurance, and safety rating requirements affect carrier selection
- [Customs & Clearance Context](../customs-clearance-context/) -- CBP, C-TPAT, and export control regulations govern customs operations
- [Logistics Standards](../logistics-standards/) -- Many standards exist to satisfy regulatory requirements
- [Anti-Corruption Layer](../anti-corruption-layer/) -- Government system integrations use ACL adapters
- [Value Objects](../value-objects/) -- Regulatory concepts (HOS windows, hazmat classifications) are modeled as value objects

## References

- Federal Motor Carrier Safety Administration. "Federal Motor Carrier Safety Regulations." 49 CFR Parts 390-399. US Department of Transportation.
- Federal Motor Carrier Safety Administration. "Hazardous Materials Regulations." 49 CFR Parts 171-180. US Department of Transportation.
- US Customs and Border Protection. "Customs Regulations of the United States." 19 CFR.
- US Customs and Border Protection. "C-TPAT Program Guidelines." CBP, current edition.
- Bureau of Industry and Security. "Export Administration Regulations." 15 CFR Parts 730-774.
- European Parliament. "General Data Protection Regulation (GDPR)." Regulation (EU) 2016/679.
- European Parliament. "Common Rules for Access to the International Road Haulage Market." Regulation (EC) 1072/2009.
- International Chamber of Commerce. "Incoterms 2020." ICC, 2019.
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
