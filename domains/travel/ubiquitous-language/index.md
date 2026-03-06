# Travel - Ubiquitous Language

Reservation: A confirmed booking for one or more travel services (flights, hotels, car rentals), serving as the primary aggregate root in the Reservation Context.

Passenger: An individual traveler associated with a reservation, identified by name, travel documents, and loyalty membership.

Itinerary: A structured travel plan comprising one or more segments organized in chronological order for a trip.

Segment: A single leg or component of an itinerary, such as one flight between two cities or one hotel stay.

Flight Instance: A specific occurrence of a scheduled flight on a particular date, identified by carrier, flight number, and departure date.

PNR: Passenger Name Record -- a unique alphanumeric code assigned by a GDS or airline to identify a reservation.

Fare Class: A letter code designating the booking class and associated fare rules for a flight segment (e.g., Y for full economy, B for discounted business).

Fare Basis Code: An alphanumeric code that encodes the fare class, restrictions, season, and other pricing attributes of a ticket.

Fare Rule: A set of conditions governing a fare, including advance purchase requirements, minimum stay, change fees, and refund eligibility.

Ticket: A travel document authorizing a passenger to travel on specified flight segments under specified fare conditions.

E-Ticket: An electronic ticket record stored in the airline's system, replacing physical paper tickets.

Coupon: A portion of a ticket corresponding to a single flight segment, consumed upon travel.

GDS: Global Distribution System -- an intermediary platform (Amadeus, Sabre, Travelport) connecting travel providers with agents and booking engines.

Availability: The real-time inventory of seats, rooms, or vehicles offered for sale at a given price for a given date.

Inventory: The allocation of seats or rooms that an airline or hotel makes available for sale through various distribution channels.

Booking Class: A letter designation within a cabin that determines pricing and availability, distinct from the physical cabin class.

Cabin Class: The physical service class on an aircraft (Economy, Premium Economy, Business, First).

Ancillary Service: An optional add-on service sold alongside the primary travel product, such as seat selection, baggage, meals, or lounge access.

Revenue Management: The practice of dynamically adjusting prices and inventory allocations to maximize revenue based on demand forecasting.

Yield: Revenue per passenger-kilometer or per available seat-kilometer, a key metric in airline pricing.

Currency: A value object representing a monetary amount with its ISO 4217 currency code (e.g., USD, EUR, GBP).

Tax: A government-imposed charge applied to a travel transaction, identified by tax code and jurisdiction.

Date Range: A value object representing a start date and end date, used for travel periods, booking windows, and validity periods.

Check-In: The process by which a passenger confirms their intent to travel, receives a boarding pass, and is assigned a seat.

Boarding Pass: A travel document issued at check-in authorizing a passenger to board a specific flight.

Seat Assignment: The allocation of a specific seat on a flight to a passenger, which may be pre-assigned or assigned at check-in.

Frequent Flyer: A loyalty program membership that accrues miles or points based on travel activity and provides tier-based benefits.

Loyalty Tier: A status level within a frequent flyer program (e.g., Silver, Gold, Platinum) that determines benefits and priority.

Codeshare: An arrangement where a flight operated by one airline is marketed and sold under another airline's flight number.

Interline: An agreement between airlines allowing passengers to travel on multiple carriers under a single ticket and itinerary.

Alliance: A group of airlines that cooperate on codeshares, frequent flyer reciprocity, and coordinated schedules (e.g., Star Alliance, oneworld, SkyTeam).

Connection: A transfer between two flight segments at an intermediate airport, requiring minimum connection time validation.

Minimum Connection Time: The minimum time required between arriving and departing flights at an airport to allow a passenger to make the connection.

Stopover: A deliberate break in a journey at an intermediate point, typically defined as a stay exceeding 24 hours on international itineraries.

Open Jaw: An itinerary where the departure city of the return differs from the arrival city of the outbound, or vice versa.

Round Trip: An itinerary departing from and returning to the same origin city.

One Way: An itinerary with travel in a single direction from origin to destination with no return.

Refund: The return of payment to a passenger for an unused or partially used ticket, subject to fare rules.

Void: The cancellation of a ticket within the void period (typically 24 hours of issuance) with full reversal of payment.

Exchange: The reissuance of a ticket for different travel dates, routes, or fare classes, potentially incurring change fees and fare differences.

Waitlist: A queue of passengers seeking confirmation on a sold-out booking class, cleared based on priority and availability.

No-Show: A passenger who fails to appear for a confirmed flight without prior cancellation.

IATA: International Air Transport Association -- the trade association setting industry standards for ticketing, messaging, and settlement.

NDC: New Distribution Capability -- an IATA standard enabling airlines to distribute rich content and ancillaries through a modern XML/JSON API.

BSP: Billing and Settlement Plan -- an IATA system for financial settlement between airlines and travel agents.

AIRIMP: ATA/IATA Reservations Interline Message Procedures -- the standard for inter-airline messaging about reservations and ticketing.

PCI DSS: Payment Card Industry Data Security Standard -- the security standard governing the handling of credit card data in payment processing.

Anti-Corruption Layer: A translation boundary that converts external GDS or airline system data formats into the internal domain model's ubiquitous language.

Order: A confirmed purchase encompassing one or more reservations and associated payment records.

Invoice: A financial document issued to the traveler or agent detailing charges, taxes, and payment terms for a booking.
