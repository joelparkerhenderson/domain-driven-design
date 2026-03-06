# Metering & Billing Context in Energy

## Overview

The Metering & Billing Context is the bounded context responsible for measuring electricity consumption and generation at customer premises, validating and storing meter data, applying tariff rates to calculate charges, generating invoices, and managing customer accounts. It serves as the revenue interface between the utility and its customers, translating physical measurements into financial obligations.

## Relevance to Energy

Revenue collection is fundamental to utility operations. The Metering & Billing Context manages the data pipeline from millions of smart meters through validation, estimation, and editing (VEE) to accurate customer invoices. It handles the complexity of diverse tariff structures (time-of-use, tiered, demand-based, real-time pricing), net metering for DER owners, and billing adjustments for outages and demand response participation.

## Bounded Context Boundaries

**Owns**: Meter aggregate, CustomerAccount aggregate, BillingCycle aggregate, Invoice aggregate, tariff application, meter data validation, billing calculation

**Does not own**: Physical meter installation (field operations), wholesale market prices (Trading & Market Context), tariff design and approval (Regulatory & Grid Context), DER generation data (Renewable Integration Context)

**Upstream contexts**: Transmission & Distribution Context (outage data for billing credits), Trading & Market Context (wholesale prices for large customers), Regulatory & Grid Context (tariff schedules), Renewable Integration Context (DER generation for net metering)

**Downstream contexts**: None -- this is a leaf context that produces customer-facing outputs

## Core Capabilities

### Meter Data Management

Meter data management collects, validates, and stores interval consumption data from the meter fleet.

- Receive automated meter readings from smart meters via advanced metering infrastructure (AMI)
- Process manual reads from conventional meters entered by field technicians
- Apply validation, estimation, and editing (VEE) rules to incoming data
- Validate readings against physical constraints (non-negative consumption, maximum demand limits)
- Detect anomalous patterns indicating meter malfunction, tampering, or data transmission errors
- Estimate missing readings using historical consumption patterns and weather data
- Store validated interval data at the granularity required for tariff application (typically 15-minute or hourly)

### Tariff Application

Tariff application determines the applicable rate structure and calculates charges for each customer.

- Determine the customer's tariff class based on service type (residential, commercial, industrial), voltage level, and usage characteristics
- Apply time-of-use rates that vary by hour and season
- Calculate tiered energy charges with increasing rates at higher consumption levels
- Compute demand charges based on the customer's peak power draw during the billing period
- Apply fixed charges, minimum bills, and customer charges
- Calculate taxes, surcharges, and regulatory fees
- Handle tariff transitions when a customer changes rate class or when new tariffs take effect

### Billing Cycle Management

Billing cycle management coordinates the monthly process of generating customer invoices.

- Define billing cycle schedules aligned with meter reading routes
- Aggregate validated consumption data for each customer's billing period
- Apply the appropriate tariff to calculate total charges
- Generate invoices with itemized charges, consumption summary, and payment instructions
- Handle prorated billing for service start, stop, and mid-cycle rate changes
- Process billing adjustments for outage credits, demand response credits, and error corrections

### Net Metering

Net metering manages the billing credits for customers who export electricity from distributed energy resources.

- Receive DER generation data from the Renewable Integration Context
- Calculate net consumption by subtracting exported generation from total consumption
- Apply net metering credits at the applicable rate (full retail, avoided cost, or time-varying)
- Roll over excess credits to future billing periods as allowed by the net metering tariff
- Track annual true-up and credit expiration per regulatory requirements

## Key Aggregates and Entities

- **Meter** (aggregate root): A metering device measuring electricity flow, identified by MeterId
- **CustomerAccount** (aggregate root): A customer's service account, identified by AccountId
- **BillingCycle** (aggregate root): A billing period with associated readings and charges
- **Invoice** (aggregate root): A customer-facing billing document, identified by InvoiceId
- **MeterReading** (value object within Meter): A consumption or generation measurement at a point in time
- **TariffRate** (value object): The rate structure applicable to a customer
- **BillingPeriod** (value object): The date range of a billing cycle
- **DemandCharge** (value object): A charge based on peak power draw

## Domain Events Produced

- **MeterReadingReceived**: A consumption or generation measurement has been recorded
- **MeterCommunicationLost**: A smart meter has stopped communicating
- **MeterCommunicationRestored**: A previously offline meter has resumed communication
- **BillingCycleCompleted**: A billing cycle has been calculated and invoiced
- **TariffUpdated**: A tariff rate schedule has been modified
- **NetMeteringCreditApplied**: A DER owner has been credited for exported electricity
- **InvoiceGenerated**: A customer invoice has been created

## Domain Events Consumed

- **ServiceRestored** (from Transmission & Distribution Context): Outage end time for billing credit calculation
- **OutageDetected** (from Transmission & Distribution Context): Outage start time for tracking affected customers
- **MarketPricePublished** (from Trading & Market Context): Wholesale prices for real-time pricing customers
- **TariffApproved** (from Regulatory & Grid Context): New or updated tariff schedules
- **DERGenerationRecorded** (from Renewable Integration Context): DER output for net metering

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Metering & Billing is one of six core bounded contexts
- [Transmission & Distribution Context](../transmission-distribution-context/) -- Source of outage data for billing adjustments
- [Trading & Market Context](../trading-market-context/) -- Source of wholesale price data
- [Regulatory & Grid Context](../regulatory-grid-context/) -- Source of tariff schedules and net metering rules
- [Renewable Integration Context](../renewable-integration-context/) -- Source of DER generation data
- [Domain Events](../domain-events/) -- Produces metering and billing events

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- ANSI C12 Standards. "American National Standards for Electricity Metering." ANSI, various years.
- Green Button Standard. "Green Button Data: Common Format for Energy Usage Information." Green Button Alliance.
- NAESB REQ/WEQ Standards. "North American Energy Standards Board Business Practice Standards." NAESB.
