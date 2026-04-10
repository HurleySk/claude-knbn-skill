# knbn — Claude Code Kanban Board Skills

A [Claude Code](https://claude.ai/claude-code) skill set for tracking work across multiple Claude Code CLI instances via the [knbn VS Enterprise extension](https://github.com/HurleySk/knbn).

## Prerequisites

The **knbn VS Enterprise extension** must be installed and the knbn tool window must be open (View → knbn Board) for the skills to work. The extension runs an HTTP aggregator on `localhost:9090` that these skills communicate with.

## Installation

### Via Marketplace

```bash
claude plugin marketplace add HurleySk/claude-plugins-marketplace
claude plugin install knbn
```

### Direct

```bash
claude plugin add HurleySk/claude-knbn-skill
```

## Skills

| Command | Description |
|---------|-------------|
| `/knbn "Title"` | Register this session as working on a named work item |
| `/knbn:knbn-join` | Smart-join an existing work item based on your context |
| `/knbn:knbn-status "msg"` | Post a manual status update to your card |
| `/knbn:knbn-status` | Auto-generate a status update from conversation context |

## How It Works

1. Start the knbn board in VS Enterprise (View → knbn Board)
2. In any Claude Code CLI session, run `/knbn "Auth System"` to create a card
3. In another session, run `/knbn:knbn-join` — the agent analyzes context and links to the right card
4. Run `/knbn:knbn-status` periodically to keep the board updated

Cards appear on a three-column kanban board: **Active** | **Waiting** | **Done**.

## License

MIT
