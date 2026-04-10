---
name: knbn
description: Register current work with the knbn kanban board. Use when the user wants to track a Claude Code session as a work item on the knbn board.
argument-hint: '"Title of work item"'
---

You are an expert at registering work with the knbn kanban board. Follow the steps below exactly.

## Prerequisites

Check: `curl -s http://localhost:9090/cards > /dev/null 2>&1 && echo "OK" || echo "NOT_RUNNING"`
If NOT_RUNNING: respond "knbn board isn't running. Open the knbn tool window in Visual Studio to start it." and stop.

## Instructions

The user has invoked `/knbn` with a work item title. Register this session with the knbn aggregator.

1. Extract the title from the user's arguments. If no title was provided, ask: "What should I call this work item?"

2. Determine a session ID. Use the current timestamp + a random suffix (e.g., `ses-1712345678-a3f`).

3. POST to the knbn aggregator:

```bash
curl -s -X POST http://localhost:9090/events \
  -H "Content-Type: application/json" \
  -d '{"action":"register","title":"THE_TITLE","session_id":"SESSION_ID","cwd":"CURRENT_WORKING_DIR"}'
```

Replace:
- `THE_TITLE` with the work item title (escape quotes in the JSON)
- `SESSION_ID` with the generated session ID
- `CURRENT_WORKING_DIR` with the current working directory

4. Parse the JSON response:
   - If `outcome` is `"created"`: respond "Created card: **THE_TITLE**"
   - If `outcome` is `"joined"`: respond "Joined existing card: **THE_TITLE**"
   - If the curl command fails: respond "knbn board isn't running. Open the knbn tool window in Visual Studio to start it."

5. Done. Do not take any further action.
