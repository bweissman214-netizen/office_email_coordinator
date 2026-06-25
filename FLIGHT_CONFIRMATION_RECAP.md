# Flight Confirmation Recap

## Overview

Once the flight is confirmed and selected, the system automatically generates a comprehensive recap email to the primary contact with all engagement and travel details.

**Triggers on:** When flight selection is confirmed (e.g., "Let's do this please: Option 2 - American Airlines")

## How It Works

1. **Detect flight confirmation** from organizer's reply
2. **Extract flight choice** from confirmation message
3. **Gather all event details** from email thread:
   - Organizer contact
   - Event location
   - Event date
   - Budget confirmed
   - Selected flight details
   - Accommodation requirements link
4. **Create comprehensive draft** to charlie@charlieharary.com
5. **Include all relevant details** in one recap email

## Detection Logic

**Triggers on flight selection message containing:**
- "Let's do this please"
- "Option 1" or "Option 2" mention
- Flight details (airline, times, price)

**Extracts:**
- Selected airline
- Departure time and airport
- Arrival time and airport
- Flight duration
- Aircraft type
- Return flight details
- Total price

## Draft Template

**Recipient:** Primary contact (speaker/principal)

**Subject:** "Speaking Engagement Recap - [LOCATION], [DATE]"

**Body includes:**
- Greeting to recipient
- Event details (location, date, organizer, budget)
- Requirements confirmed status
- Complete flight details:
  - Outbound flight (airline, departure, arrival, duration, aircraft)
  - Return flight (airline, departure, arrival, duration)
  - Total cost
- Link to accommodations document
- Call to action: "Please confirm receipt"

## Test Case (Verified)

**Flight Confirmation:**
- Organizer: blake@backpackvc.com
- Selected: Option 2 - American Airlines
- Details: "Let's do this please: 10:15 AM departure, 2:05 PM arrival, $1,185"

**Recap Generated:**
- **Draft ID:** `r-1752573055207998733`
- **To:** Primary contact (principal speaker)
- **Subject:** Speaking Engagement Recap - Los Angeles, July 18th, 2026
- **Date Created:** June 25, 2026 at 7:30 PM
- **Status:** ✅ Draft created and ready to send

**Content Verified:**
- ✅ Event location: Los Angeles
- ✅ Event date: July 18th, 2026
- ✅ Organizer: blake@backpackvc.com
- ✅ Budget: $5,000
- ✅ Airline: American Airlines
- ✅ Departure: 10:15 AM JFK
- ✅ Arrival: 2:05 PM LAX
- ✅ Return: 7:30 PM LAX → 4:15 AM+1 JFK
- ✅ Price: $1,185 roundtrip
- ✅ Accommodations link included
- ✅ Charlie requested to confirm

## Definition of Done

✅ Draft created with all event and flight details
✅ Sent to primary contact (principal speaker)
✅ Recipient responds with confirmation to proceed

## Implementation Notes

- Automatically extracts all details from email thread
- Creates single comprehensive recap (not multiple emails)
- Includes both outbound and return flight info
- Links to accommodation requirements
- Ready for Blake to send to Charlie
- Asks for explicit confirmation from Charlie

## Future Enhancements

- Auto-send to Charlie (optional)
- Calendar invite generation
- Travel itinerary PDF
- Hotel booking integration
- Ground transportation coordination
- Pre-event briefing materials
