# Regulatory Compliance in Energy

## Overview

Regulatory compliance in the energy domain encompasses the legal and governance requirements imposed by federal and state agencies, reliability organizations, and environmental regulators that shape how energy systems operate, how markets function, and how customers are served. In DDD terms, regulatory compliance is a cross-cutting concern that influences domain model design across all bounded contexts, imposing constraints on aggregates, defining required domain events, and mandating specific data retention and reporting obligations.

## Relevance to Energy

The energy industry operates under one of the most extensive regulatory frameworks of any sector. Compliance failures can result in substantial financial penalties (NERC violations can carry fines up to $1 million per day per violation), operational restrictions, and public safety consequences. A DDD approach treats regulatory requirements as first-class domain concepts: compliance obligations are modeled as aggregates, regulatory deadlines drive domain events, and reporting formats shape value object design.

## Federal Energy Regulatory Commission (FERC)

FERC regulates the interstate transmission of electricity, natural gas, and oil, and oversees wholesale energy markets.

### Key FERC Regulations

- **FERC Order 888/889**: Open access to transmission, establishing the framework for wholesale energy markets and the Open Access Same-Time Information System (OASIS)
- **FERC Order 2000**: Formation of Regional Transmission Organizations (RTOs) to manage transmission and administer wholesale markets
- **FERC Order 2222**: Participation of distributed energy resource aggregations in wholesale markets, allowing DER aggregators to compete alongside traditional generators
- **FERC Order 841**: Energy storage participation in wholesale markets, removing barriers to storage participation in capacity, energy, and ancillary service markets
- **FERC Form 1**: Annual report of major electric utilities, requiring detailed financial and operational data
- **FERC Form 714**: Annual electric balancing authority area and planning area report
- **Electric Quarterly Report (EQR)**: Quarterly report of energy transactions, including contracts and wholesale sales

### DDD Impact

- The Regulatory & Grid Context maintains ComplianceObligation aggregates for each FERC requirement
- The Trading & Market Context must support EQR reporting for all wholesale transactions
- The Generation Context must report unit-level operational data for Form 1 and Form 714
- FERC Order 2222 directly impacts the Renewable Integration Context's DER aggregation capabilities

## North American Electric Reliability Corporation (NERC)

NERC develops and enforces reliability standards for the bulk power system in North America.

### Key NERC Reliability Standards

- **BAL Standards (Resource and Demand Balancing)**: Requirements for maintaining balance between generation and load, including area control error limits and frequency response obligations
- **FAC Standards (Facilities Design, Connections, and Maintenance)**: Requirements for transmission facility ratings, interconnection, and maintenance
- **IRO Standards (Interconnection Reliability Operations)**: Requirements for real-time reliability coordination
- **MOD Standards (Modeling, Data, and Analysis)**: Requirements for power system modeling and data exchange
- **PRC Standards (Protection and Control)**: Requirements for protection system design, testing, and coordination
- **TOP Standards (Transmission Operations)**: Requirements for real-time transmission operations
- **TPL Standards (Transmission Planning)**: Requirements for long-term transmission system planning
- **CIP Standards (Critical Infrastructure Protection)**: Cybersecurity requirements (see [Energy Standards](../energy-standards/) for details)

### DDD Impact

- The Regulatory & Grid Context maintains a compliance tracking system for all applicable NERC standards
- The Transmission & Distribution Context must capture data required by FAC, PRC, and TOP standards
- The Generation Context must capture data required by BAL and MOD standards
- NERC CIP requirements affect all contexts that touch bulk electric system cyber assets

## Environmental Regulations

Environmental regulations govern emissions, land use, water use, and waste management for energy facilities.

### EPA Regulations

- **Clean Air Act**: Emissions limits for SO2, NOx, particulate matter, and mercury from fossil fuel generators
- **Greenhouse Gas Reporting Program**: Mandatory reporting of CO2 and other greenhouse gas emissions for facilities above threshold levels
- **Clean Water Act**: Requirements for cooling water intake and discharge at generation stations
- **RCRA**: Hazardous waste management for coal ash and other generation byproducts

### Carbon Regulations

- **Regional Greenhouse Gas Initiative (RGGI)**: Cap-and-trade program for power sector CO2 emissions in northeastern US states
- **California Cap-and-Trade**: Economy-wide emissions trading program administered by CARB
- **Carbon tax programs**: Jurisdictional programs that set a price per ton of CO2 emitted
- **DDD impact**: The Generation Context must track emissions data per unit and per fuel type. Carbon allowance management may be modeled in the Trading & Market Context alongside energy trading.

## Renewable Portfolio Standards (RPS)

State-level mandates requiring utilities to procure a specified percentage of their electricity from renewable sources.

- Each state defines its own RPS targets, eligible technologies, and compliance timelines
- Compliance is demonstrated through renewable energy certificate (REC) retirement
- Some states have technology-specific carve-outs (e.g., solar-specific mandates)
- Penalties for non-compliance include alternative compliance payments (ACPs)

### DDD Impact

- The Renewable Integration Context tracks REC generation and transfers
- The Regulatory & Grid Context tracks RPS obligations and retirement requirements
- The Trading & Market Context may trade RECs on the market to meet obligations
- ComplianceObligation aggregates model each state's RPS requirement with its target, deadline, and evidence

## State Public Utility Commission (PUC) Regulations

State PUCs regulate retail electricity rates, service quality, and customer protections.

- **Rate cases**: Proceedings where utilities request approval for changes to retail tariffs
- **Service quality standards**: Requirements for reliability (SAIDI, SAIFI), call center response, and billing accuracy
- **Net metering rules**: State-specific rules for DER customer billing credits
- **Consumer protection**: Requirements for billing practices, disconnection procedures, and customer complaint handling

### DDD Impact

- The Metering & Billing Context must implement tariff structures approved by the PUC
- The Transmission & Distribution Context must track reliability indices per PUC requirements
- The Regulatory & Grid Context manages rate case filings and service quality reporting

## Compliance Modeling in DDD

### Compliance as a First-Class Domain Concept

- Model each regulatory obligation as a ComplianceObligation aggregate with standard, requirement, deadline, status, and evidence
- Track compliance lifecycle: identified, in-progress, evidence-collected, filed, confirmed, or violated
- Publish ComplianceViolationDetected events when standards are breached
- Maintain audit trails through domain events for regulatory examination

### Reporting as a Domain Concern

- Model required reports as domain concepts with their filing deadlines, data requirements, and submission procedures
- Use ACLs to translate internal domain data into regulatory reporting formats (NERC, FERC, EPA)
- Track report submission status and regulatory acknowledgment

## Relationships to Other Topics

- [Energy Standards](../energy-standards/) -- Standards implement regulatory requirements
- [Regulatory & Grid Context](../regulatory-grid-context/) -- Owns compliance tracking and reporting
- [Anti-Corruption Layer](../anti-corruption-layer/) -- ACLs translate to regulatory reporting formats
- [Domain Events](../domain-events/) -- Compliance events trigger cross-context actions
- [Bounded Contexts](../bounded-contexts/) -- Regulatory requirements constrain all bounded contexts
- [Renewable Integration Context](../renewable-integration-context/) -- REC tracking for RPS compliance

## References

- FERC. "Federal Energy Regulatory Commission." https://www.ferc.gov/
- NERC. "North American Electric Reliability Corporation Reliability Standards." https://www.nerc.com/
- EPA. "Clean Air Act, Clean Water Act, Greenhouse Gas Reporting." https://www.epa.gov/
- RGGI. "Regional Greenhouse Gas Initiative." https://www.rggi.org/
- DSIRE. "Database of State Incentives for Renewables and Efficiency." https://www.dsireusa.org/
- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Hempling, Scott. "Regulating Public Utility Performance." American Bar Association, 2013.
