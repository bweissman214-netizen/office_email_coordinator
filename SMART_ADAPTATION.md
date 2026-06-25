# Smart Adaptation Layer

## Overview
Enhanced system that interprets vague input, makes intelligent assumptions, and asks targeted clarification questions.

## Adaptation Rules

### Location Handling
**Problem:** Vague locations ("East Coast", "New York area")
**Solution:**
- Map regions to primary airports (East Coast → NYC/Boston area → ask which)
- "New York area" → default to JFK, but offer alternatives (LGA, EWR)
- Ask for specific city if completely vague

**Examples:**
- "East Coast" → "Which city? Boston, NYC, or DC?"
- "California" → Default to LAX, confirm
- "New York" → Default to JFK, offer LGA/EWR if needed

### Date Validation
**Problem:** Invalid or vague dates ("December 32nd", "sometime next month")
**Solution:**
- Validate date exists (reject Dec 32, suggest nearest valid date)
- Parse vague dates: "next month" → ask for specific week/date
- Offer to check calendar availability

**Examples:**
- "December 32nd" → "Did you mean December 31st or January 1st?"
- "Sometime next month" → "Would early August or mid-August work?"

### Budget Validation
**Problem:** Budget too low ($300 for $1K+ flights)
**Solution:**
- Compare budget to estimated flight cost
- If low: "Your budget is $300 but flights are ~$1K+. Confirm or increase?"
- Flag but don't reject—let user decide

**Examples:**
- $300 budget → "Flights JFK-NYC cost $400+. Increase budget or proceed?"
- $15K budget → "Covers flights + expenses. Confirm?"

### Multi-Location Handling
**Problem:** Multiple cities in one request (Boston, NYC, DC)
**Solution:**
- Ask for primary location
- Offer to handle as separate bookings
- Don't assume—clarify intent

**Examples:**
- Multi-city → "Should we focus on Boston (July 10) or book separate events for each city?"
- "Tour" → "Is this 1 event with travel, or multiple 1-day events?"

### Missing Data Strategy
**Problem:** Email has location but no date, or date but no budget
**Solution:**
- Ask for next critical piece (don't ask for all at once)
- Make reasonable assumptions but confirm them
- Order questions by importance: Date > Location > Budget

**Clarification Order:**
1. **Date** (most critical for scheduling)
2. **Location** (needed for flights)
3. **Budget** (needed for feasibility)

## Response Template (Enhanced)

**For incomplete/vague input:**

"Thanks for the inquiry. Just a few clarifications:

1. **Date**: You mentioned [X]. Is [DATE] confirmed, or would [ALTERNATIVE] work better?
2. **Location**: You said [VAGUE]. Did you mean [SPECIFIC]? We can arrange flights from JFK to [AIRPORT].
3. **Budget**: What's your budget for this engagement? (For reference, NYC flights are ~$400-1K depending on dates)

Let me know and we'll move forward!"

---

## Implementation Priority

**Phase 1 (Now):** Location & date handling
**Phase 2:** Budget validation  
**Phase 3:** Multi-location logic
**Phase 4:** Predictive defaults based on historical data

## Testing Against Broken Cases

1. **East Coast (vague)** → Ask which city (Boston/NYC/DC?)
2. **New York $300** → Warn: flights exceed budget, confirm?
3. **Multi-city** → Ask for primary location
4. **Generic** → Ask for date first, then location, then budget
5. **December 32nd** → Suggest Dec 31 or Jan 1, confirm

---

## Benefits

✅ Handles 80% of messy real-world input
✅ Doesn't reject—adapts and clarifies
✅ User-friendly with specific suggestions
✅ Reduces back-and-forth with intelligence
✅ Fallback to manual review for edge cases
