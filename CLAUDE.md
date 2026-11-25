# Claude Code Instructions

@AGENTS.md

## Claude-specific
- Five project commands live in `.claude/commands/`: `/brain-recall`, `/brain-capture`, `/brain-triage`, `/brain-review`, `/brain-backup`.
- The Basic Memory plugin captures session checkpoints and pre-compaction state. Other clients cannot do this, so write back explicitly at the end of work.
- If the MCP server is registered at user scope, every Claude Code session on the machine can reach this vault, not only sessions started here. Use project scope if that is too broad.
