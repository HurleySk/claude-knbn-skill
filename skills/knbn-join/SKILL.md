---
name: knbn-join
description: Smart-join an existing knbn work item based on current context. Use when the user wants to link their Claude Code session to an existing kanban card.
argument-hint: ""
---

You are an expert at analyzing context and joining the right knbn work item. Follow the steps below exactly.

## Prerequisites

Check: `curl -s http://localhost:9090/cards > /dev/null 2>&1 && echo "OK" || echo "NOT_RUNNING"`
If NOT_RUNNING: respond "knbn board isn't running. Open the knbn tool window in Visual Studio to start it." and stop.

## Instructions

1. Fetch active cards from the knbn aggregator:

```bash
curl -s http://localhost:9090/cards
```

2. Parse the JSON response to get the list of cards.

3. **If no cards exist:**
   - Say: "No active work items on the board. Want me to create one? What should I call it?"
   - Wait for the user's response, then POST a register event:
     ```bash
     curl -s -X POST http://localhost:9090/events \
       -H "Content-Type: application/json" \
       -d '{"action":"register","title":"USER_TITLE","session_id":"SESSION_ID","cwd":"CWD"}'
     ```

4. **If cards exist:**
   - Look at your current context: working directory, any recent user prompts, files you've been working with
   - Compare against the cards' titles and project directories
   - **Confident match** (same directory or clearly related title): Say "This looks like it relates to **[card title]** — joining." and POST:
     ```bash
     curl -s -X POST http://localhost:9090/events \
       -H "Content-Type: application/json" \
       -d '{"action":"join","card_id":"CARD_ID","session_id":"SESSION_ID","cwd":"CWD"}'
     ```
   - **Uncertain** (multiple possible matches): Present the options: "I see these active work items: [list with numbers]. Which one is this related to, or should I create a new one?"
   - **No reasonable match**: Say "None of the active work items seem related to what we're doing. What should I call this work?" Then POST a register event.

5. Done. Do not take any further action.
