# Setup Instructions

## Prerequisites

1. **Claude Code** installed and configured
2. **Gmail MCP** enabled in Claude Code
3. **Two Gmail accounts authenticated:**
   - **Inquiry receiver:** `blake@backpackvc.com` (receives speaking requests)
   - **Responder:** `bweissman214@gmail.com` (sends budget vetting form)

## Step 1: Verify Gmail MCP Access

Test access to both accounts from Claude Code:
```
Ask Claude: "Search blake@backpackvc.com for unread emails"
Ask Claude: "Search bweissman214@gmail.com for draft emails"
```

Both should return results. If you get permission errors, enable Gmail MCP in Claude Code settings.

## Step 2: Set Up Your Accounts

Configure which account is which:
- **Inquiry Inbox:** blake@backpackvc.com (speaking requests arrive here)
- **Response Account:** bweissman214@gmail.com (sends budget vetting drafts from here)

Both must be authenticated in Gmail MCP.

## Step 3: Test the Workflow

**Manual test first (no automation):**

1. **Send a test email** from blake@backpackvc.com to bweissman214@gmail.com with subject "Charlie Speaking Request"
2. **Ask Claude in Claude Code:**
   ```
   Find the unread email from blake@backpackvc.com in bweissman214@gmail.com inbox.
   Create a draft reply asking "What is your budget for this engagement?"
   ```
3. **Verify the draft** appears in bweissman214@gmail.com's Drafts folder
4. **Check content** matches the budget vetting template

## Step 4: Set Up Continuous Monitoring

Once manual test passes, automate with `/loop 5m`:

```
Ask Claude: Check blake@backpackvc.com for unread speaking-related emails. 
For each one found, create a draft from bweissman214@gmail.com asking 
"What is your budget for this engagement?" Report all draft IDs created.
```

This will run every 5 minutes automatically.

## Step 5: Deploy to Production

When ready to use with Charlie's office email:

1. **Update email addresses:**
   - Change `blake@backpackvc.com` → actual office inquiry email
   - Change `bweissman214@gmail.com` → actual responder email

2. **Update response signature** if needed:
   - Keep or modify "Charlie Harary's Office"

3. **Re-test** with new email addresses

4. **Deploy** `/loop` on production emails

## Troubleshooting

### Draft not created
- **Check:** Is the inquiry email actually in the inbox?
- **Check:** Does it mention speaking, event, engagement, or similar keywords?
- **Fix:** Ask Claude to search with broader keywords

### Drafts in wrong account
- **Check:** Which account is authenticated as default in Gmail MCP?
- **Fix:** Ensure responses are created from the responder account, not inquiry account

### No emails received
- **Check:** Are inquiry emails going to the right account?
- **Check:** Is the email address correct?
- **Fix:** Verify email addresses in all communications

### Permission errors
- **Check:** Both Gmail accounts authenticated in Claude Code?
- **Fix:** Re-authenticate Gmail MCP connection

## Testing Checklist

- [ ] Gmail MCP enabled and both accounts authenticated
- [ ] Can search blake@backpackvc.com successfully
- [ ] Can create drafts in bweissman214@gmail.com successfully
- [ ] Test email sent from blake@backpackvc.com to bweissman214@gmail.com
- [ ] Draft created asking for budget
- [ ] Draft appears in Drafts folder
- [ ] Draft content is accurate
- [ ] `/loop 5m` runs without errors

---

Once checklist is complete, the workflow is ready for production use.
