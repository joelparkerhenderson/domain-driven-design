# Regulatory Compliance in Fulfillment

## Overview

Regulatory compliance in fulfillment encompasses the legal, governmental, and industry regulations that constrain how goods are stored, handled, transported, and delivered. In Domain-Driven Design, regulatory requirements shape the domain model by introducing invariants on aggregates, validation rules on value objects, and constraints on domain services. Compliance is not an afterthought bolted onto the system -- it is a first-class domain concern that must be modeled explicitly in the ubiquitous language and enforced by the domain logic.

## Relevance to Fulfillment

Fulfillment operations are subject to a broad range of regulations depending on the products being shipped, the geographies involved, and the customers being served. Shipping lithium batteries requires hazmat declarations. Crossing international borders requires customs documentation. Handling consumer personal data requires privacy protections. Selling age-restricted products requires verification. Failure to comply results in shipment seizures, fines, legal liability, carrier account suspensions, and reputational damage.

## Customs and Import/Export Regulations

### Export Controls

- **Export classification**: Products must be classified under export control lists (e.g., Commerce Control List in the US, EU Dual-Use Regulation) to determine if an export license is required
- **Denied party screening**: Before shipping internationally, the recipient must be screened against government denied-party lists (US BIS Entity List, SDN List, EU sanctions lists)
- **Country restrictions**: Certain destinations may be subject to comprehensive sanctions or embargoes that prohibit or restrict shipments
- **Domain modeling**: The Order Processing Context validates international orders against export control rules before allowing fulfillment to proceed

### Import Regulations

- **Customs declarations**: International shipments require customs documentation including commercial invoice, packing list, and customs declaration form (CN22/CN23 for postal, commercial invoice for courier)
- **Harmonized System (HS) codes**: Each product must have an HS code for customs classification, which determines applicable duty rates and import restrictions
- **Country of origin**: Products must declare their country of origin for tariff determination and trade agreement eligibility
- **De minimis thresholds**: Shipments below a country's de minimis value threshold may be exempt from duties and simplified customs processing
- **Domain modeling**: Value objects for HSCode, CountryOfOrigin, and CustomsDeclaration enforce format and completeness rules; the Shipping & Carrier Context generates customs documentation as part of label generation for international shipments

### Free Trade Agreements

- **Preferential tariff rates**: Products qualifying under free trade agreements (USMCA, EU FTAs) may receive reduced duty rates
- **Certificate of origin**: Documentation proving that goods qualify for preferential treatment under a trade agreement
- **Domain modeling**: Trade agreement eligibility is evaluated during customs documentation generation

## Hazardous Materials (Hazmat) Regulations

### Classification and Handling

- **UN classification**: Hazardous materials are classified into nine classes under the UN Recommendations on the Transport of Dangerous Goods (explosives, gases, flammable liquids, flammable solids, oxidizers, toxic substances, radioactive materials, corrosives, miscellaneous)
- **Lithium battery regulations**: Lithium-ion and lithium-metal batteries have specific packing, labeling, and documentation requirements under IATA DGR (air), IMDG Code (sea), and DOT 49 CFR (ground)
- **Quantity limits**: Maximum allowable quantities per package and per shipment vary by hazmat class, packing group, and transport mode
- **Domain modeling**: Product master data includes hazmat classification attributes; the PackingService enforces segregation rules, quantity limits, and packaging requirements; the Shipping & Carrier Context generates required hazmat documentation and placards

### Labeling and Documentation

- **Hazmat labels and markings**: Packages containing hazardous materials must display the appropriate diamond-shaped hazard labels, proper shipping name, and UN number
- **Shipper's declaration**: A formal declaration documenting the hazardous materials in a shipment, required by carriers and regulatory agencies
- **Safety data sheets (SDS)**: Must accompany certain hazmat shipments and be available for warehouse personnel handling the products
- **Domain modeling**: Hazmat labeling requirements are enforced during label generation; the Shipment aggregate validates that all required hazmat documentation is present before dispatch

## Consumer Protection Regulations

### Product Safety

- **Recall management**: When a product is recalled, the fulfillment system must stop shipping affected items, quarantine warehouse stock, and support the return of products already delivered
- **Age-restricted products**: Alcohol, tobacco, certain medications, and other age-restricted products require age verification at delivery and may have additional licensing requirements
- **Product labeling**: Consumer goods must meet labeling requirements for the destination country (ingredient lists, safety warnings, language requirements)
- **Domain modeling**: Product restrictions are checked during order validation; age verification requirements are passed to the carrier as delivery instructions; recall flags halt allocation and trigger quarantine workflows

### Consumer Rights

- **Return rights**: Many jurisdictions mandate minimum return windows (e.g., 14 days under the EU Consumer Rights Directive for distance sales)
- **Delivery commitments**: Consumer protection laws may impose penalties for late delivery or failure to deliver within committed timeframes
- **Pricing transparency**: Shipping costs must be clearly communicated and not exceed quoted amounts
- **Domain modeling**: Return policies in the Returns & Exceptions Context encode jurisdiction-specific mandatory return windows; delivery commitment tracking is modeled in the Delivery & Tracking Context

## Data Privacy Regulations

### Personal Data in Fulfillment

Fulfillment systems process significant amounts of personal data: customer names, shipping addresses, phone numbers, email addresses, and delivery preferences. This data is subject to privacy regulations.

- **GDPR (General Data Protection Regulation)**: Applies to personal data of EU residents regardless of where the fulfillment operation is located. Requires lawful basis for processing, data minimization, purpose limitation, and data subject rights (access, rectification, erasure).
- **CCPA/CPRA (California Consumer Privacy Act / California Privacy Rights Act)**: Grants California consumers rights regarding their personal information, including the right to know, delete, and opt out of sale.
- **Other jurisdictions**: LGPD (Brazil), PIPEDA (Canada), POPIA (South Africa), and other national privacy laws impose similar requirements.

### Domain Modeling for Privacy

- **Data minimization**: The domain model should only include personal data necessary for fulfillment. A FulfillmentOrder needs a shipping address but does not need the customer's date of birth (unless age verification is required).
- **Purpose limitation**: Personal data collected for fulfillment should not be repurposed for marketing without separate consent.
- **Data retention**: Define retention periods for personal data in fulfillment records; implement automated purging or anonymization after the retention period expires.
- **Right to erasure**: Support deletion or anonymization of personal data upon customer request, while preserving the ability to comply with other legal requirements (tax records, product safety records).
- **Cross-border data transfer**: When fulfillment involves warehouses or carriers in different jurisdictions, ensure personal data transfers comply with applicable data transfer mechanisms (Standard Contractual Clauses, adequacy decisions).

## Environmental Regulations

### Packaging Waste

- **Extended Producer Responsibility (EPR)**: In the EU and other jurisdictions, companies that introduce packaging into the market must register with packaging compliance schemes and contribute to recycling costs
- **Packaging material restrictions**: Regulations may restrict the use of certain packaging materials (polystyrene, single-use plastics) in specific jurisdictions
- **Domain modeling**: Packaging selection in the PackingService considers regulatory constraints on materials by destination

### Carbon Reporting

- **Emissions tracking**: Regulations and customer expectations increasingly require tracking and reporting the carbon footprint of fulfillment and shipping operations
- **Domain modeling**: Carrier selection may factor in emissions data when environmental optimization is a business requirement

## Compliance in the Domain Model

Regulatory compliance is encoded in the domain model through several mechanisms.

- **Aggregate invariants**: A Shipment aggregate enforces that hazmat documentation is present before dispatch. A FulfillmentOrder enforces that denied-party screening is completed before international shipment.
- **Value object validation**: An HSCode value object validates format and classification. A CustomsDeclaration value object ensures all required fields are populated.
- **Domain service rules**: The PackingService enforces hazmat segregation. The ReturnAuthorizationService enforces jurisdiction-specific return windows.
- **Domain events**: ComplianceCheckCompleted, HazmatViolationDetected, and RecallQuarantineInitiated events enable audit trails and exception handling.

## Relationships to Other Topics

- [Bounded Contexts](../bounded-contexts/) -- Compliance rules may span multiple contexts but are enforced locally within each
- [Value Objects](../value-objects/) -- Standards-based identifiers (HS codes, UN numbers) are modeled as validated value objects
- [Domain Services](../domain-services/) -- Compliance checks are performed by domain services during validation and packing
- [Fulfillment Standards](../fulfillment-standards/) -- Technical standards (GS1, EDI) support regulatory compliance documentation
- [Shipping & Carrier Context](../shipping-carrier-context/) -- Customs and hazmat documentation is generated during shipping
- [Returns & Exceptions Context](../returns-exceptions-context/) -- Consumer protection return rights are enforced during return processing

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." O'Reilly, 2021.
- United Nations. "UN Recommendations on the Transport of Dangerous Goods: Model Regulations." United Nations, current edition.
- European Parliament. "General Data Protection Regulation (GDPR)." Regulation (EU) 2016/679. 2016.
- World Customs Organization. "Harmonized Commodity Description and Coding System." WCO, current edition.
- International Chamber of Commerce. "Incoterms 2020." ICC, 2019.
