# Setup Instructions

## Prerequisites

1. **Claude Code** installed and configured
2. **Gmail MCP** enabled in Claude Code
3. **Email account** set up and authenticated (test: `blake@backpackvc.com`)
4. **Gmail Drafts folder** accessible

## Step 1: Verify Gmail MCP Access

Test that Gmail MCP is working:
```
Ask Claude: "Search blake@backpackvc.com for any unread emails"
```

You should see a list of unread emails. If you get permission errors, enable Gmail MCP in Claude Code settings.

## Step 2: Run Initial Scan

First run to establish baseline:

**⚠️ IMPORTANT:** Ask Claude directly in Claude Code (NOT via spawned agents):

```
Search blake@backpackvc.com inbox for unread emails with keywords: speak, speaking, 
presentation, event, panel, appearance, engagement, invitation, keynote, moderator.

For each match found:
1. Verify it's actually a speaking REQUEST (not just mentioning the word)
2. Use Gmail create_draft tool to create a reply with:
   - To: [original sender email]
   - Subject: Re: [original subject]
   - Body: "Thank you for your inquiry about Charlie speaking at your event. To help 
     us determine if this is a good fit for his calendar, could you please share:\n\n
     What is your budget for this engagement?\n\nOnce we receive this information, 
     we'll be able to evaluate whether we can move forward.\n\nBest regards,\n
     Charlie Harary's Office"

3. Report the draft ID for each one created

This will scan the current inbox and create drafts for any existing speaking requests.
```

**Why direct, not via agent:** Agents cannot access MCP tools like Gmail's create_draft. You must call Gmail tools directly from Claude Code.

## Step 3: Set Up Continuous Monitoring (Optional)

To run the agent continuously every 5 minutes:

```
/loop 5m
```

Then provide the agent prompt:
```
Monitor blake@backpackvc.com for speaking-related email requests. Search for 
unread emails with keywords: speak, speaking, presentation, event, panel, 
appearance, engagement, invitation, keynote, moderator. For each match, 
create a DRAFT reply asking about budget. Avoid creating duplicate drafts 
for the same thread.
```

## Step 4: Monitor Results

1. Check your **Gmail Drafts folder** regularly
2. Review created drafts for accuracy
3. Edit and send as needed
4. Check agent logs for any errors

## Step 5: Transfer to Production

When ready to use with Charlie's actual office email:

1. **Update email address:**
   - Change `blake@backpackvc.com` → `charlie@[company-email].com`

2. **Update response signature:**
   - Change "Charlie Harary's Office" to official signature if needed

3. **Re-test:** Send a test email to the new address and verify agent responds

4. **Deploy:** Set up permanent `/loop` monitoring on the production email

## Troubleshooting

### Agent doesn't detect speaking emails
- **Check:** Does the email subject or body contain at least one keyword?
- **Fix:** Email might be in spam/promotions folder. Check all folders.
- **Fix:** Manually verify email content matches keywords

### Too many false positives
- **Check:** Are emails being matched that shouldn't be?
- **Example:** Emails about attending an event (not speaking at one)
- **Fix:** Update keywords to be more specific, or add exclusion keywords like `-attend`

### No drafts being created
- **Check:** Are there unread emails in the inbox?
- **Check:** Do they mention speaking-related keywords?
- **Fix:** Run agent with verbose logging to see what's being searched

### Permission errors
- **Check:** Gmail MCP is enabled in Claude Code settings
- **Check:** Email account is authenticated
- **Fix:** Re-authenticate Gmail connection

## Email Keywords Reference

**Detected as speaking requests:**
- speak, speaking, speaker
- talk, talking, discussion
- presentation, present
- appearance, appear
- event, conference, panel
- engagement, engage
- invite, invitation
- keynote, moderator

**To customize:** Update the agent prompt with different keywords

## Testing Checklist

- [ ] Claude Code has Gmail MCP enabled
- [ ] Email account authenticated and accessible
- [ ] Initial scan runs without errors
- [ ] Test email with "speaking" keyword is created
- [ ] Agent detects test email
- [ ] Draft reply is created successfully
- [ ] Draft is visible in Gmail Drafts folder
- [ ] No duplicate drafts created for same thread
- [ ] Loop setup works (optional)

---

Once all steps are complete, the agent is ready for production use.
