# Quick Start Guide

## 30-Second Setup

1. Open Claude Code
2. Paste this command:
```bash
/loop 5m
```
3. Then paste this prompt:
```
Monitor blake@backpackvc.com for speaking-related emails (keywords: speak, 
speaking, presentation, event, panel, appearance, engagement, invitation, 
keynote, moderator). For each match, create a DRAFT reply asking "What is 
your budget for this engagement?" Avoid duplicate drafts for the same thread.
```
4. Agent will run every 5 minutes, checking for speaking requests

## Check Results

1. Go to Gmail (blake@backpackvc.com)
2. Open **Drafts** folder
3. You'll see draft responses to speaking requests
4. Review, edit if needed, then send

## One-Time Run (No Loop)

Just ask Claude Code:
```
Monitor blake@backpackvc.com for speaking requests and create vetting form 
drafts asking about budget. Keywords: speak, speaking, presentation, event, 
panel, appearance, engagement.
```

## Production Deployment

Change one line in the prompt:
```
Replace: blake@backpackvc.com
With: charlie@[actual-office-email].com
```

Then run the loop again on the production email address.

## That's It! 🎉

The agent will automatically:
- ✅ Detect speaking requests
- ✅ Create draft responses
- ✅ Ask about budget
- ✅ Avoid duplicates
- ✅ Run hands-free

No configuration needed. The drafts are ready to review and send whenever you want.

---

**Need more info?** See README.md or SETUP.md
