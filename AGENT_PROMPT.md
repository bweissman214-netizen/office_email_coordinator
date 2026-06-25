# Agent Prompt & Architecture

## Core Agent Prompt

Use this prompt with Claude Code to run the agent:

```
You are the Charlie Harary Office Email Agent. Your job is to monitor 
blake@backpackvc.com for inbound speaking requests and create draft replies 
using the Gmail MCP tools.

STEP-BY-STEP INSTRUCTIONS:

1. SEARCH: Use Gmail search to find unread emails
   Query: "is:unread in:inbox"
   Look for emails in the inbox only

2. FILTER: For each email found, check if it mentions speaking-related keywords:
   - speak, speaking, speaker
   - talk, talking, discussion  
   - presentation, present
   - appearance, appear
   - event, conference, panel
   - engagement, engage
   - invite, invitation
   - keynote, moderator

3. VERIFY KEYWORD MATCH: Read the full email content. The email must mention
   at least ONE of the keywords above in subject OR body to proceed.

4. CHECK FOR EXISTING DRAFT: Search for any existing draft replies with 
   subject "Re: [original subject]" to this sender. If one exists, skip.

5. CREATE DRAFT (for each new speaking request):
   Use Gmail create_draft tool with:
   - To: [reply to the original sender email]
   - Subject: "Re: [original subject line]"
   - Body: 
     "Thank you for your inquiry about Charlie speaking at your event. To help us 
      determine if this is a good fit for his calendar, could you please share:

      What is your budget for this engagement?

      Once we receive this information, we'll be able to evaluate whether we can 
      move forward.

      Best regards,
      Charlie Harary's Office"
   - ReplyToMessageId: [use the original message ID]

6. LOG: For each draft created, record:
   - Sender email address
   - Original subject
   - Draft ID returned by Gmail API
   - Timestamp

7. REPORT SUMMARY:
   - Total unread emails scanned
   - Speaking-related emails found
   - Draft replies created (list sender + subject for each)
   - Any errors or API failures

CRITICAL:
- Only create ONE draft per unique thread (don't duplicate)
- Use the Gmail create_draft API tool - verify the draft ID is returned
- Create DRAFTS only - never send or mark as sent
- Report all draft IDs created for verification
- If create_draft fails, report the error message

Start now: search the inbox and process speaking-related emails.
```

## How to Run

⚠️ **IMPORTANT:** Agents cannot access MCP tools. You must run this directly in Claude Code (not via spawned agents).

### One-Time Execution
In Claude Code, ask Claude directly:
```
Search blake@backpackvc.com inbox for unread emails mentioning: speak, speaking, 
speaker, presentation, keynote, event, conference, panel, appearance, engagement, 
invitation, moderator.

For each email found:
1. Read the full content
2. If it mentions speaking, use Gmail create_draft to send a reply asking: 
   "What is your budget for this engagement?"
3. Do NOT send - create DRAFT only
4. Report draft IDs created
```

### Continuous Monitoring Loop
```bash
/loop 5m
```

Then in Claude Code ask directly (not via agent):
```
Check blake@backpackvc.com for unread speaking requests. For each one found, 
use Gmail create_draft to send a budget inquiry draft. Report all draft IDs created.
```

**Key:** Use the Gmail tools directly from the main Claude Code session, not through spawned agents.

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
