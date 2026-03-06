# Value Objects in Finance

## Overview

A value object is an immutable domain object defined entirely by its attributes rather than by a unique identity. Two value objects with the same attributes are considered equal and interchangeable. Value objects have no identity, no lifecycle, and no side effects -- they simply represent a descriptive aspect of the domain.

## Relevance to Finance

Finance is rich with concepts that are naturally modeled as value objects. An amount of money is defined by its numeric value and currency -- two instances of $100.00 USD are interchangeable. An interest rate of 5.25% is the same regardless of which loan it applies to. Value objects bring precision, type safety, and domain clarity to financial calculations.

## Key Concepts

### Immutability

Value objects cannot be changed after creation. To represent a different value, create a new instance. This is critical in finance where audit trails require knowing the exact values at each point in time.

### Equality by Attributes

Two Money objects with the same amount and currency are equal. There is no need to track "which" $100 -- they are all equivalent.

### Self-Validation

Value objects validate their own invariants at construction time. A Money object rejects negative amounts for certain use cases, an IBAN validates its check digits, and an InterestRate rejects values outside a reasonable range.

## Finance Value Objects

### Money

The most fundamental financial value object, combining an amount with a currency.

- **Attributes**: Amount (decimal), Currency (ISO 4217 code)
- **Invariants**: Amount must have appropriate decimal precision for the currency; currency must be a valid ISO 4217 code
- **Operations**: Add, subtract, multiply by scalar, allocate proportionally, convert currencies
- **Design note**: Never represent money as a bare floating-point number. Always pair the amount with a currency. Use decimal types to avoid floating-point rounding errors.

### IBAN (International Bank Account Number)

- **Attributes**: CountryCode, CheckDigits, BBAN (Basic Bank Account Number)
- **Invariants**: Passes MOD-97 check digit validation; country code is valid ISO 3166-1 alpha-2
- **Format**: Up to 34 alphanumeric characters (e.g., DE89370400440532013000)

### BIC (Bank Identifier Code)

- **Attributes**: InstitutionCode, CountryCode, LocationCode, BranchCode (optional)
- **Invariants**: 8 or 11 characters; valid country code
- **Format**: e.g., DEUTDEFF (Deutsche Bank Frankfurt)

### InterestRate

- **Attributes**: Rate (decimal), Basis (annual, monthly, daily), DayCountConvention (Actual/360, Actual/365, 30/360)
- **Invariants**: Rate must be within a reasonable range; basis must be specified
- **Operations**: Convert between bases, calculate interest for a period

### CreditScore

- **Attributes**: Score (integer), ScoreModel (FICO, VantageScore, internal), AssessmentDate
- **Invariants**: Score within valid range for the model (e.g., 300-850 for FICO)

### SecurityIdentifier

- **Attributes**: Identifier value, IdentifierType (ISIN, CUSIP, SEDOL, Ticker)
- **Invariants**: ISIN must be 12 characters with valid check digit; CUSIP must be 9 characters

### DateRange

- **Attributes**: StartDate, EndDate
- **Invariants**: EndDate must be on or after StartDate
- **Usage**: Fiscal periods, loan terms, reporting windows

### Address

- **Attributes**: Street, City, State/Province, PostalCode, Country
- **Usage**: Customer addresses, branch locations, registered addresses

### Percentage

- **Attributes**: Value (decimal)
- **Invariants**: Contextually bounded (e.g., 0-100 for ownership stakes)
- **Operations**: Apply to amount, compare

### AmortizationScheduleEntry

- **Attributes**: PaymentNumber, PaymentDate, PrincipalPortion (Money), InterestPortion (Money), RemainingBalance (Money)
- **Invariants**: PrincipalPortion + InterestPortion = TotalPayment; RemainingBalance is non-negative

## Design Guidelines

1. **Prefer value objects over primitives**: Use Money instead of decimal, IBAN instead of string, CreditScore instead of integer
2. **Make them immutable**: No setters. Any "modification" returns a new instance
3. **Implement equality by value**: Two Money(100, USD) objects must be equal
4. **Validate at construction**: An IBAN with invalid check digits should never exist in the system
5. **Encapsulate operations**: Currency conversion belongs on Money, not in a utility class

## Relationships to Other Topics

- [Entities](../entities/) -- Entities contain value objects as attributes
- [Aggregates](../aggregates/) -- Value objects are internal members of aggregates
- [Domain Events](../domain-events/) -- Events carry value objects as payload data
- [Finance Standards](../finance-standards/) -- Standards define formats that value objects encapsulate (IBAN, BIC, ISIN)
- [Ubiquitous Language](../ubiquitous-language/) -- Value object names come from the ubiquitous language

## References

- Evans, Eric. "Domain-Driven Design: Tackling Complexity in the Heart of Software." Chapter 5: A Model Expressed in Software. Addison-Wesley, 2003.
- Vernon, Vaughn. "Implementing Domain-Driven Design." Chapter 6: Value Objects. Addison-Wesley, 2013.
- Millett, Scott & Tune, Nick. "Patterns, Principles, and Practices of Domain-Driven Design." Chapter 17: Domain Model Building Blocks. Wrox, 2015.
- Fowler, Martin. "Patterns of Enterprise Application Architecture." Chapter 14: Money Pattern. Addison-Wesley, 2002.
- ISO 13616. "Financial Services -- International Bank Account Number (IBAN)." ISO, 2020.
