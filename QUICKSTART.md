# Quick Start Guide

## 1-Minute Setup

**Two accounts needed:**
1. `blake@backpackvc.com` - receives speaking inquiries
2. `bweissman214@gmail.com` - sends budget vetting responses

Both must be authenticated in Claude Code Gmail MCP.

## Test It (30 seconds)

1. Send email from blake@backpackvc.com to bweissman214@gmail.com asking "Does Charlie have a date to speak?"
2. In Claude Code, ask:
   ```
   Create a draft from bweissman214@gmail.com to blake@backpackvc.com 
   asking "What is your budget for this engagement?"
   ```
3. Check bweissman214@gmail.com Drafts folder
4. Draft should be there ready to send ✅

## Automate It

Once tested, set up continuous monitoring:

```bash
/loop 5m
```

Then ask Claude:
```
Check blake@backpackvc.com for unread speaking requests.
For each one found, create a draft from bweissman214@gmail.com 
asking "What is your budget for this engagement?"
Report all draft IDs created.
```

## That's It! 🎉

The workflow will automatically:
- ✅ Detect speaking inquiries
- ✅ Create draft responses asking for budget
- ✅ Run every 5 minutes
- ✅ Ready to review and send

---

**Need more help?** See README.md or SETUP.md
