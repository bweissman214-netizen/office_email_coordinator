# Budget Response Detection Logic

## Workflow

When a budget response is received, the system should:

1. **Search** for unread emails TO blake@backpackvc.com with budget-related keywords
2. **Identify** if they're responses to the budget question
3. **Extract** sender address
4. **Create draft** from bweissman214@gmail.com with Google Doc link
5. **Record** to avoid duplicates

## Detection Keywords

Email must contain at least ONE of:
- "budget" (any case)
- "$" (dollar sign)
- "thousand", "hundred", "million"
- "cost", "expense", "fund"
- "amount", "price"
- Numbers followed by "k" or "K" (e.g., "10k")

## Draft Template

**From:** bweissman214@gmail.com
**To:** [original inquirer email]
**Subject:** Re: Charlie Speaking Request - Accommodations
**Body:**
```
Thank you for providing your budget information. Here are Charlie's 
speaking accommodations and requirements:

https://docs.google.com/document/d/1gTlbYQocuLAKsPSaxVNW_ufBi8kPHW3CvPokfSKX1G4/edit?usp=sharing

Please review the document and let us know if you have any questions 
or need any clarifications.

Best regards,
Charlie Harary's Office
```

## Implementation

Search query:
```
to:blake@backpackvc.com is:unread (budget OR $ OR cost OR amount OR "speaking request")
```

For each result:
1. Read full message
2. Check if contains budget keywords
3. Extract sender email
4. Create draft with Google Doc link
5. Record draft ID and sender to avoid duplicates

## Success Criteria

✅ Detects budget mention in response
✅ Creates draft with correct Google Doc link
✅ Draft is to original inquirer
✅ No duplicate drafts for same inquiry
✅ Reports draft ID for verification
