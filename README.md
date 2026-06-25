# Charlie Harary Office Email Agent

An AI-powered email agent that automatically responds to inbound speaking requests with a vetting form, asking about budget to qualify opportunities.

## Overview

This agent monitors an email inbox (configured for `blake@backpackvc.com` during testing) for speaking-related requests and creates draft responses asking prospective event organizers for their budget. This helps the office quickly qualify opportunities without manual back-and-forth.

**Status:** Tested and working. Ready for production deployment to Charlie Harary's office email.

## How It Works

1. **Monitors inbox** for unread emails
2. **Detects speaking requests** using keyword matching:
   - speak, speaking, speaker
   - talk, talking, discussion
   - presentation, present
   - appearance, appear
   - event, conference, panel
   - engagement, engage
   - invite, invitation
   - keynote, moderator
3. **Creates draft replies** (for review before sending) asking: *"What is your budget for this engagement?"*
4. **Avoids duplicates** by not responding twice to the same thread

## Quick Start

### Prerequisites
- Claude Code with Gmail MCP enabled
- Access to the target email account
- Email account already authenticated in Gmail MCP

### Running the Agent (Testing)

In Claude Code, use:
```bash
/loop 5m [run speaking request detection on target inbox]
```

This will check for new speaking requests every 5 minutes and create draft responses.

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

✅ **Agent tested and verified working (June 25, 2026)**

- Test email sent: `bweissman214@gmail.com` → `blake@backpackvc.com`
- Subject: "Charlie Speaking Engagement"
- Agent detected speaking keyword ✓
- Draft created successfully ✓
- Draft ID: `r-8934955211285023310`
- No duplicates created ✓
- **Status:** Tested with real Gmail MCP API. Works end-to-end.

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
