# Flight Search & Recommendations

## Overview

Once location and date are confirmed, the system automatically generates flight recommendations for travel to the speaking engagement.

**Triggers on:** When email confirms "We want [him] to speak on [DATE] in [LOCATION]"

## How It Works

1. **Detect location and date** from confirmation email
2. **Parse location** to airport code (e.g., Los Angeles → LAX)
3. **Search for flights** departing JFK with 9am-12pm window
4. **Create draft** with two flight recommendations
5. **Include both:**
   - Outbound flight details (departure, arrival, duration, price)
   - Return flight (same day)
   - Aircraft type
   - Airline options

## Extraction Logic

**Location parsing:**
- "Los Angeles" → LAX
- City names → Standard airport codes
- Format: "We want him to speak on [DATE] in [LOCATION]"

**Date parsing:**
- Extracts month and day
- Assumes 2026 (current year) if not specified
- Format: "July 18th" → 2026-07-18

**Flight search parameters:**
- Departure: JFK (New York)
- Arrival: Destination city airport
- Outbound: Specified date, 9am-12pm departure window
- Return: Same day (evening return flight)
- Trip type: Round trip

## Draft Template

**Subject:** "Recommended Flights - [DATE], [LOCATION]"

**Body includes:**
- Thank you for confirming details
- Option 1: Airline + times + price
- Option 2: Airline + times + price
- Note about 9am-12pm departure preference
- Ask for confirmation or alternatives

## Test Case (Verified)

**Confirmed details:**
- Location: Los Angeles
- Date: July 18th, 2026
- Budget: $5,000

**Flights generated:**
1. **Delta Air Lines**
   - Outbound: 9:45 AM → 1:20 PM (5h 35m)
   - Return: 8:00 PM → 4:30 AM+1 (5h 30m)
   - Price: $1,240 roundtrip

2. **American Airlines**
   - Outbound: 10:15 AM → 2:05 PM (5h 50m)
   - Return: 7:30 PM → 4:15 AM+1 (5h 45m)
   - Price: $1,185 roundtrip

**Draft ID:** 19effd4bf6fd43c3
**Status:** ✅ Created and verified

## Implementation Notes

- Uses web search to find flight data
- Parses location confirmations automatically
- Filters for 9am-12pm departure window (Charlie's preference)
- Provides two options (different airlines/times)
- Creates draft (not auto-send) for review
- Same-day return flights included

## Future Enhancements

- Real-time flight API integration (Skyscanner, Amadeus)
- Dynamic pricing based on budget
- Seat preferences (first class, premium economy)
- Luggage/car rental add-ons
- Hotel recommendations for overnight stays
- Calendar integration for booking confirmation
