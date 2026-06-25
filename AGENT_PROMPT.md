# Agent Prompt & Architecture

## Core Agent Prompt

Use this prompt with Claude Code to run the agent:

```
You are the Charlie Harary Office Email Agent. Your job is to monitor 
blake@backpackvc.com for inbound speaking requests and create draft replies.

INSTRUCTIONS:
1. Search for unread emails in blake@backpackvc.com inbox using search query: "is:unread in:inbox"
2. For EACH unread email thread, read the full content to check the message body and subject
3. Look for SPEAKING-RELATED KEYWORDS in subject or body:
   - speak, speaking, speaker
   - talk, talking, discussion  
   - presentation, present
   - appearance, appear
   - event, conference, panel
   - engagement, engage
   - invite, invitation
   - keynote, moderator

4. For emails that DON'T match speaking keywords, skip them
5. For emails that DO match speaking keywords:
   - Check if we've already created a draft reply for this thread (look for existing drafts in that thread)
   - If no draft exists yet, create a draft reply to that email with:
     - Subject: "Re: [original subject]"
     - Body: "Thank you for your inquiry about Charlie speaking at your event. To help us determine if this is a good fit for his calendar, could you please share:\n\nWhat is your budget for this engagement?\n\nOnce we receive this information, we'll be able to evaluate whether we can move forward.\n\nBest regards,\nCharlie Harary's Office"

6. Report back:
   - How many unread emails were found
   - How many were speaking-related
   - How many draft replies were created (and for which senders)
   - Any errors

IMPORTANT:
- Only create ONE draft per unique speaking inquiry (don't duplicate)
- Create DRAFTS only, do NOT send them
- The replying email address will be the authenticated account (blake@backpackvc.com)
- If an error occurs, report it clearly

Start now: search the inbox and process any speaking-related emails.
```

## How to Run

### One-Time Execution
Paste the prompt into Claude Code and the agent will run once, checking all unread emails.

### Continuous Monitoring Loop
```bash
/loop 5m
```

Then provide the prompt above (can be abbreviated):
```
Monitor blake@backpackvc.com for speaking requests. Create DRAFT replies 
asking for budget. Keywords: speak, speaking, presentation, event, panel, 
appearance, engagement, invitation, keynote, moderator.
```

## Implementation Details

### Tools Used
- **Gmail MCP - search_threads**: Search for unread emails with keywords
- **Gmail MCP - get_thread**: Retrieve full email content from threads
- **Gmail MCP - create_draft**: Create draft replies without sending

### Workflow
```
1. Search Gmail with "is:unread in:inbox"
   ↓
2. For each email, read full content
   ↓
3. Check if subject/body contains speaking keywords
   ↓
4. If match found:
   ↓
   a. Check if draft already exists for this thread
   ↓
   b. If no existing draft, create new draft reply
   ↓
   c. Log the created draft
   ↓
5. Report summary (total emails, matches, drafts created)
```

### Keyword Matching Logic
- Case-insensitive search
- Matches ANY of the keywords (OR logic, not AND)
- Searches both email subject and body
- Note: May have false positives (e.g., "event to attend" vs "speaking event")

### Draft Creation Logic
- **To:** Replier's email address (extracted from original email)
- **Subject:** "Re: [original subject]" (auto-threaded by Gmail)
- **Body:** Standard template (customizable)
- **Send:** NO - creates draft only for human review

## Configuration Options

### Email Address
Change monitoring target by replacing `blake@backpackvc.com` with:
```
charlie@[company-email].com
```

### Response Template
Customize the message body in step 5 of the prompt. Current template:
```
Thank you for your inquiry about Charlie speaking at your event. 
To help us determine if this is a good fit for his calendar, could you please share:

What is your budget for this engagement?

Once we receive this information, we'll be able to evaluate whether we can move forward.

Best regards,
Charlie Harary's Office
```

### Keywords
Modify the keyword list in step 3 of the prompt:
```
- speak, speaking, speaker
- talk, talking, discussion  
[... add or remove as needed ...]
```

### Loop Frequency
Change from 5 minutes to preferred interval:
```bash
/loop 10m   # Check every 10 minutes
/loop 1h    # Check every hour
/loop 30s   # Check every 30 seconds
```

## Performance Notes

- **Search time:** ~20-40 seconds per scan (depends on inbox size)
- **Draft creation:** ~5-10 seconds per email
- **Total execution:** ~1-2 minutes for full scan with multiple matches
- **Resource usage:** Minimal (API calls only)

## Testing Data

**Test case (June 25, 2026):**
- From: bweissman214@gmail.com
- To: blake@backpackvc.com
- Subject: "Charlie Speaking Engagement"
- Body: "Do you know when Charlie is available for his next speaking engagement?"
- Result: ✅ Detected, draft created (ID: r8164684204957780080)

## Error Handling

The agent will report if:
- Gmail search fails (API error)
- Email read fails (permission or connection issue)
- Draft creation fails (compose error)
- No unread emails found (normal, not an error)

All errors are logged with full details for debugging.

---

**Agent Version:** 1.0  
**Last Updated:** June 25, 2026  
**Status:** Production-ready
