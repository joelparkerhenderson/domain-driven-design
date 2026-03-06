# Value Objects in Fulfillment

## Overview

A value object is a domain object defined entirely by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. Value objects are immutable -- once created, their state does not change. If a different value is needed, a new value object is created. In fulfillment, value objects represent the measurements, identifiers, labels, and descriptive data that characterize entities and aggregates.

## Relevance to Fulfillment

Fulfillment operations rely heavily on precise measurements, standardized identifiers, and structured data. A package's weight is 2.5 kg regardless of which package it belongs to -- two packages weighing 2.5 kg have the same weight. An address is defined by its components (street, city, state, zip, country), not by some abstract identity. These concepts are naturally modeled as value objects, providing type safety, validation, and expressiveness throughout the domain model.

## Key Concepts

### Immutability

Value objects cannot be modified after creation. To change an address, you create a new Address value object rather than modifying the existing one. This eliminates shared-state bugs and makes reasoning about the domain simpler.

### Equality by Value

Two value objects are equal if all their attributes are equal. Two `Address` objects with the same street, city, state, zip, and country are the same address. There is no "AddressId" to distinguish them.

### Self-Validation

Value objects validate their own data upon creation. A `Weight` value object rejects negative values. A `TrackingNumber` verifies its format matches the carrier's pattern. Invalid value objects cannot exist.

### Side-Effect-Free Behavior

Value objects may contain methods, but those methods must not produce side effects. A `Dimensions` object can calculate volume; a `Weight` object can convert units. These operations return new value objects rather than modifying state.

## Fulfillment Value Objects

### Address

**Attributes**: Street1, Street2, City, State/Province, PostalCode, Country, IsResidential, IsVerified

**Validation rules**:

- PostalCode must match the format for the specified country
- Country must be a valid ISO 3166-1 alpha-2 code
- State/Province must be valid for the specified country
- Street1 is required

**Behavior**:

- `isInternational(originCountry)`: Determines if the address requires international shipping
- `toCarrierFormat(carrier)`: Formats the address for a specific carrier's requirements
- `equals(other)`: Value-based equality comparison

**Used by**: FulfillmentOrder (shipping address), Warehouse (facility address), Shipment (destination), ReturnRequest (return-to address)

### Weight

**Attributes**: Value (decimal), Unit (kg, lb, oz, g)

**Validation rules**:

- Value must be positive
- Unit must be a recognized weight unit

**Behavior**:

- `convertTo(targetUnit)`: Returns a new Weight in the target unit
- `add(other)`: Returns a new Weight representing the sum, normalizing units
- `exceedsLimit(maxWeight)`: Checks against carrier or warehouse weight limits
- `toDimensionalWeight(dimensions, factor)`: Calculates dimensional weight for carrier billing

**Used by**: Package (actual weight), Shipment (total weight), CarrierRate (weight-based pricing)

### Dimensions

**Attributes**: Length, Width, Height (all decimal), Unit (in, cm, mm)

**Validation rules**:

- All dimension values must be positive
- Unit must be a recognized length unit

**Behavior**:

- `volume()`: Returns calculated volume (Length x Width x Height)
- `convertTo(targetUnit)`: Returns new Dimensions in the target unit
- `fitsIn(container)`: Checks if these dimensions fit within a container's dimensions
- `girth()`: Calculates girth (2 x (Width + Height) + Length) used by carriers for oversized determination

**Used by**: Package (package dimensions), BinLocation (storage dimensions), Product (product dimensions for pack optimization)

### TrackingNumber

**Attributes**: Value (string), Carrier (carrier identifier), Format (barcode format)

**Validation rules**:

- Value must match the carrier's tracking number format (e.g., UPS 1Z pattern, FedEx 12/15/20/22 digit format)
- Carrier must be a recognized carrier
- Check digit validation where applicable

**Behavior**:

- `toTrackingUrl()`: Generates the carrier-specific tracking URL
- `toBarcode()`: Generates barcode representation for label printing

**Used by**: Shipment (primary tracking), Package (package-level tracking)

### ShippingLabel

**Attributes**: TrackingBarcode, DestinationAddress, ReturnAddress, ServiceLevel, Weight, CarrierLogo, RoutingCode, LabelFormat (ZPL, PDF, PNG), RawLabelData

**Validation rules**:

- All required fields must be present
- LabelFormat must be supported by the printing infrastructure
- RoutingCode must match carrier specifications

**Behavior**:

- `toPrintableFormat(printerType)`: Converts label data to the appropriate format for the target printer

**Used by**: Package (one label per package)

### CarrierRate

**Attributes**: CarrierId, ServiceLevel, BaseRate (Money), Surcharges (collection of Surcharge), TotalRate (Money), Currency, TransitDays, RateExpiresAt

**Validation rules**:

- TotalRate must equal BaseRate plus the sum of all Surcharges
- TransitDays must be positive
- RateExpiresAt must be in the future at time of creation

**Behavior**:

- `isExpired()`: Checks if the rate quote has expired
- `compareTo(other)`: Compares rates for rate shopping

**Used by**: Shipment (selected rate), rate shopping results

### ServiceLevel

**Attributes**: Code (enum: GROUND, TWO_DAY, OVERNIGHT, EXPRESS, FREIGHT, WHITE_GLOVE), Description, MaxTransitDays

**Validation rules**:

- Code must be a recognized service level
- MaxTransitDays must be consistent with the service level code

**Behavior**:

- `meetsDeadline(shipDate, requiredDeliveryDate)`: Determines if this service level can meet the delivery deadline

**Used by**: FulfillmentOrder (requested service level), Shipment (assigned service level), CarrierRate (rate-specific service level)

### Barcode

**Attributes**: Value (string), Format (CODE128, GS1-128, QR, DataMatrix), EncodedData

**Validation rules**:

- Value must be valid for the specified barcode format
- GS1 barcodes must conform to GS1 encoding standards

**Behavior**:

- `toImage(width, height)`: Generates barcode image for printing
- `encode()`: Returns the encoded barcode string

**Used by**: Package (package barcode), PickList (item barcode scanning), BinLocation (location barcode)

### Money

**Attributes**: Amount (decimal), Currency (ISO 4217 code)

**Validation rules**:

- Amount must not be negative (for most use cases)
- Currency must be a valid ISO 4217 currency code

**Behavior**:

- `add(other)`: Returns new Money with sum (currencies must match)
- `subtract(other)`: Returns new Money with difference
- `multiply(factor)`: Returns new Money scaled by factor
- `convertTo(targetCurrency, exchangeRate)`: Currency conversion

**Used by**: CarrierRate (pricing), RefundAmount, OrderLine (item value)

### DateRange

**Attributes**: StartDate, EndDate

**Validation rules**:

- StartDate must be before or equal to EndDate
- Both dates must be valid

**Behavior**:

- `contains(date)`: Checks if a date falls within the range
- `overlapsWith(other)`: Checks if two date ranges overlap
- `durationInDays()`: Returns the number of days in the range

**Used by**: PickWave (wave schedule), DeliveryWindow (estimated delivery range)

## Value Object Design Guidelines

1. **Make them immutable**: No setters, no mutation methods. All "changes" return new instances.

2. **Validate on construction**: A value object should never exist in an invalid state. Reject invalid inputs in the constructor.

3. **Implement value equality**: Override equality to compare attributes, not references.

4. **Keep them small and focused**: Each value object should represent a single cohesive concept.

5. **Use them liberally**: Prefer value objects over primitive types. Use `Weight` instead of `double`, `Address` instead of a string tuple, `Money` instead of `BigDecimal`.

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects as attributes
- [Aggregates](../aggregates/) -- Value objects live within aggregate boundaries
- [Domain Events](../domain-events/) -- Events carry value objects as payload data
- [Ubiquitous Language](../ubiquitous-language/) -- Value object names come from the ubiquitous language
- [Repositories](../repositories/) -- Value objects are persisted as part of their parent entity

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Khononov, Vlad. "Learning Domain-Driven Design." Chapter 10: Tactical Design Patterns. O'Reilly, 2021.
- Evans, Eric. "Domain-Driven Design Reference." Domain Language, Inc., 2015.
