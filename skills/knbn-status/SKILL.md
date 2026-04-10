---
name: knbn-status
description: Update the knbn board with a status message. Use when the user wants to post a progress update to their kanban card, or when auto-generating a status summary.
argument-hint: '["status message"]'
---

You are an expert at summarizing work progress for the knbn kanban board. Follow the steps below exactly.

## Prerequisites

Check: `curl -s http://localhost:9090/cards > /dev/null 2>&1 && echo "OK" || echo "NOT_RUNNING"`
If NOT_RUNNING: respond "knbn board isn't running. Open the knbn tool window in Visual Studio to start it." and stop.

## Instructions

1. **If the user provided a message** (e.g., `/knbn:knbn-status "Finished auth middleware"`):
   - Use that message as-is.

2. **If no message was provided** (just `/knbn:knbn-status`):
   - Review what you've accomplished in this conversation since your last status update (or since the start if no prior updates).
   - Summarize it in one concise sentence. Focus on what was completed or what milestone was reached, not what you're about to do.
   - Examples: "Implemented JWT validation and added route guards" or "Fixed race condition in session cleanup"

3. POST the status update:

```bash
curl -s -X POST http://localhost:9090/events \
  -H "Content-Type: application/json" \
  -d '{"action":"status","session_id":"SESSION_ID","message":"THE_MESSAGE"}'
```

Replace `SESSION_ID` with this session's ID and `THE_MESSAGE` with the status message (escape quotes in the JSON).

4. Parse the response:
   - If `outcome` is `"updated"`: respond "Status updated: **THE_MESSAGE**"
   - If `outcome` is `"not_found"`: respond "This session isn't linked to a work item. Use `/knbn` or `/knbn:knbn-join` first."
   - If curl fails: respond "knbn board isn't running. Open the knbn tool window in Visual Studio to start it."

5. Done. Do not take any further action.
