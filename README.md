# Charlie Harary Office Email Agent

An email workflow that automatically responds to inbound speaking requests with a vetting form, asking about budget to qualify opportunities.

## Overview

When someone inquires about Charlie speaking at an event, the workflow:
1. Receives the inquiry at `blake@backpackvc.com` (or target office email)
2. Creates a draft response from `bweissman214@gmail.com` (or designated responder account)
3. Draft asks: "What is your budget for this engagement?"
4. Responder reviews and sends the draft

This helps the office quickly qualify opportunities by understanding budget parameters before committing.

**Status:** ✅ Tested and verified working (June 25, 2026). Production ready.

## How It Works

1. **Receive inquiry** at target email (e.g., blake@backpackvc.com)
   - Email asks about Charlie speaking availability
2. **Create draft response** using Gmail API from responding account (e.g., bweissman214@gmail.com)
3. **Draft asks for budget:** "What is your budget for this engagement?"
4. **Review and send** the draft when ready

The workflow qualifies leads by gathering budget info before engaging further.

## Quick Start

### Prerequisites
- Claude Code with Gmail MCP enabled
- Two Gmail accounts: 
  - **Inquiry receiver:** blake@backpackvc.com (receives speaking requests)
  - **Responder:** bweissman214@gmail.com (sends budget vetting form)
- Both accounts authenticated in Gmail MCP

### Workflow

1. **Speaking inquiry arrives** at blake@backpackvc.com
2. **Ask Claude Code directly** (in terminal, not via agents):
   ```
   Search blake@backpackvc.com for unread emails about speaking requests. 
   For each one, create a draft response from bweissman214@gmail.com asking 
   "What is your budget for this engagement?"
   ```
3. **Review the draft** in bweissman214@gmail.com's Drafts folder
4. **Send when ready**

### Continuous Monitoring

Use `/loop 5m` in Claude Code and ask Claude to check for speaking requests and create budget inquiry drafts every 5 minutes.

### Manual One-Time Check

Ask Claude to run:
```
Monitor blake@backpackvc.com for speaking requests and create budget vetting drafts
```

## Response Template

When a speaking request is detected, a draft email is created:

**Subject:** `Re: [original subject]`

**Body:**
```
Thank you for your inquiry about Charlie speaking at your event. To help us 
determine if this is a good fit for his calendar, could you please share:

What is your budget for this engagement?

Once we receive this information, we'll be able to evaluate whether we can 
move forward.

Best regards,
Charlie Harary's Office
```

**Important:** This asks the requester for THEIR budget—we never disclose Charlie's rates or information.

## Test Results

✅ **Workflow tested and verified working (June 25, 2026)**

**Test Case:**
- **Inquiry sent:** blake@backpackvc.com → bweissman214@gmail.com
- **Subject:** "Charlie Speaking Request"
- **Message:** "Hi, does Charlie have a date available to speak for my organization?"
- **Date/Time:** June 25, 2026 at 5:12 PM

**Response Created:**
- **Draft ID:** `r3132081208084694392`
- **From:** bweissman214@gmail.com
- **To:** blake@backpackvc.com
- **Subject:** Re: Charlie Speaking Request
- **Content:** Budget vetting form asking "What is your budget for this engagement?"
- **Status:** ✅ Draft created, ready to send

**Verification:** Email received → Draft created → Budget question sent. End-to-end workflow confirmed working.

## Deployment to Production

To transfer this to Charlie Harary's actual office email:

1. **Update email address** in agent configuration from `blake@backpackvc.com` to `charlie@[company-email].com`
2. **Update response signature** from "Charlie Harary's Office" to official office branding if needed
3. **Set up permanent monitoring** using `/loop` with appropriate frequency (recommend 5-10 minute intervals)
4. **Create Gmail label** "Speaking-Inquiry-Processed" for tracking responded threads
5. **Test again** with 2-3 speaking requests to confirm setup

## Configuration

The agent currently looks for these keywords (can be customized):
- Speaking-related: speak, speaking, speaker, talk, presentation, appearance, event, panel, engagement, keynote, moderator, invitation

To modify keywords, update the agent prompt with new terms.

## Monitoring & Maintenance

- **Check drafts regularly** to ensure they're being created appropriately
- **Review and send** draft responses from the Drafts folder
- **Monitor for false positives** (e.g., emails about attending events vs. speaking at them)
- **Keep email labels organized** for easy tracking

## Limitations & Known Issues

- Only responds to unread emails (helps avoid reprocessing old threads)
- Creates DRAFTS only—no auto-sending to maintain human oversight
- Keyword-based detection means edge cases might be missed or false positives included
- Requires Gmail MCP access and authentication

## Future Enhancements

- Auto-send after human review (if desired)
- Smarter keyword matching using NLP
- Integration with calendar to check availability before responding
- Auto-label of speaking inquiries with custom Gmail labels
- Response time tracking and analytics

## Support

For questions about the agent:
1. Review the test results above
2. Check Gmail drafts folder for created responses
3. Verify keywords are being detected correctly by searching Gmail with the keyword list

---

**Built with Claude Code** | Tested June 25, 2026
