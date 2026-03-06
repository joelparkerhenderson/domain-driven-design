# Finance DDD - Ubiquitous Language

Account: A financial record that tracks debits, credits, and balances for a specific entity, asset, liability, or equity category.
Accrual: The recognition of revenue or expense before cash is actually received or paid, following the matching principle.
Aggregate: A cluster of domain objects treated as a single unit for data consistency, such as Account with JournalEntries and LineItems.
Aggregate Root: The single entity through which all access to an aggregate is managed, ensuring invariant enforcement.
Amortization: The systematic allocation of a loan principal or intangible asset cost over a defined schedule of payments.
Anti-Corruption Layer: A translation boundary that protects a bounded context's domain model from external system concepts and legacy systems.
Anti-Money Laundering (AML): The set of procedures, laws, and regulations designed to prevent criminals from disguising illegally obtained funds as legitimate income.
Balance Sheet: A financial statement summarizing an entity's assets, liabilities, and equity at a specific point in time.
Basel Accord: An international regulatory framework establishing minimum capital requirements and risk management standards for banks.
Bounded Context: A well-defined boundary within which a particular domain model applies and financial terms have specific meaning.
Chart of Accounts: A structured list of all accounts used by an organization to record financial transactions in the general ledger.
Clearing: The process of reconciling and matching purchase and sale transactions between counterparties before final settlement.
Collateral: An asset pledged by a borrower to secure a loan, subject to seizure if the borrower defaults on repayment obligations.
Context Map: A visual and conceptual representation of the relationships and interactions between bounded contexts.
Core Domain: The most critical subdomain that provides competitive advantage, such as risk modeling or trading algorithms.
Credit Score: A numerical value representing a borrower's creditworthiness, derived from credit history and financial behavior.
Custody: The safekeeping and administration of financial assets on behalf of clients, including settlement and corporate actions processing.
Debit: An entry on the left side of a double-entry bookkeeping system that increases asset or expense accounts and decreases liability or equity accounts.
Credit: An entry on the right side of a double-entry bookkeeping system that increases liability or equity accounts and decreases asset or expense accounts.
Default: The failure of a borrower to meet the legal obligations of a loan, triggering collection and recovery processes.
Domain Event: A record of something significant that happened in the domain, such as PaymentProcessed, TradeExecuted, or LoanDisbursed.
Domain Service: An operation that belongs to the domain but does not naturally fit within a single entity or value object.
Double-Entry Bookkeeping: An accounting method where every transaction is recorded in at least two accounts, with total debits equaling total credits.
Entity: A domain object defined by its unique identity that persists over time, such as Account, Loan, Trade, or Payment.
Escrow: A financial arrangement where a third party holds and regulates payment of funds until transaction conditions are met.
FIX Protocol: Financial Information eXchange protocol, the standard electronic messaging format for real-time securities trading.
Float: The period between when a payment is initiated and when funds are actually available to the recipient.
Fraud Detection: The process of identifying suspicious transactions or behaviors that may indicate financial crime or unauthorized activity.
General Ledger: The master accounting record containing all financial transactions of an organization, organized by chart of accounts.
IBAN: International Bank Account Number, a standardized international numbering system for identifying individual bank accounts across borders.
Integration Pattern: A design approach for managing data exchange and communication between bounded contexts.
Interest Rate: A percentage charged on borrowed money or earned on deposited funds, expressed as an annual rate.
Journal Entry: A chronological record of a financial transaction containing debits and credits that must balance.
Know Your Customer (KYC): The mandatory process of verifying the identity and assessing the risk profile of clients before establishing a business relationship.
Ledger: A book or system of records that maintains a complete history of financial transactions for an organization.
Liquidity: The degree to which an asset can be quickly converted to cash without significantly affecting its market price.
Loan Origination: The end-to-end process of creating a new loan, from application through underwriting, approval, and disbursement.
Margin: The difference between a selling price and cost, or collateral deposited to cover potential losses in leveraged trading positions.
Market Data: Real-time or historical pricing, volume, and reference data for financial instruments used in trading and valuation.
Maturity: The date on which the principal amount of a loan, bond, or other financial instrument becomes due and payable.
Money: An immutable value object representing a monetary amount paired with a specific currency code.
Net Asset Value (NAV): The total value of a fund's assets minus its liabilities, divided by the number of outstanding shares.
Nostro Account: An account that a bank holds in a foreign currency at another bank, used for international payment settlement.
Order: An instruction to buy or sell a financial instrument at a specified price or market price.
Payment: A transfer of monetary value from a payer to a payee in settlement of an obligation or purchase.
Portfolio: A collection of financial investments such as stocks, bonds, and cash equivalents held by an individual or institution.
Published Language: A well-documented shared data format used for communication between bounded contexts, such as ISO 20022 messages.
Reconciliation: The process of comparing two sets of records to verify that figures are correct and in agreement.
Repository: A mechanism for encapsulating storage, retrieval, and search behavior for aggregate roots.
Risk Assessment: The systematic process of identifying, measuring, and evaluating financial risks including credit, market, and operational risk.
Securities: Tradable financial instruments including stocks, bonds, options, and futures that represent ownership or debt obligations.
Settlement: The final exchange of securities and cash between buyer and seller to complete a trade.
Shared Kernel: A small, explicitly shared subset of the domain model that two or more bounded contexts agree to maintain together.
SWIFT: Society for Worldwide Interbank Financial Telecommunication, the global messaging network for secure international financial transactions.
Trade: A completed transaction involving the purchase or sale of a financial instrument between counterparties.
Transaction: A single financial event that affects one or more accounts and is recorded in the general ledger.
Trial Balance: A report listing the balances of all general ledger accounts to verify that total debits equal total credits.
Underwriting: The process of evaluating and assuming the risk of a financial transaction, such as a loan or insurance policy.
Value Object: An immutable domain object defined by its attributes rather than identity, such as Money, IBAN, InterestRate, or CreditScore.
Vostro Account: An account that a foreign bank holds in the local currency at a domestic bank, the mirror of a nostro account.
Wire Transfer: An electronic transfer of funds between financial institutions through a network such as Fedwire or SWIFT.
Yield: The income return on an investment, expressed as a percentage based on the investment's cost or current market value.
