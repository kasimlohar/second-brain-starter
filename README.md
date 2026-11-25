# Second Brain Starter

A local-first AI second brain built from three existing pieces:

- **Obsidian** — the human editor for a folder of Markdown files
- **Basic Memory** — an MCP server that indexes those files and exposes search/read/write tools
- **Claude Code** — the agent that queries and maintains the vault

This repository is not an application. It is a convention layer: folder layout, note grammar, agent rules, and five slash commands, plus the setup steps that connect the three tools. Your notes stay as files on disk. The index is a cache and can be rebuilt at any time.

**Status:** draft. Verified on Windows 11 with Basic Memory 1.0.0, Obsidian 1.15.1, and Claude Code. Not yet tested on macOS or Linux.

---

## Why this shape

Three invariants hold the design together:

1. **Files are the source of truth.** Markdown on disk, versioned by git. Delete the index and rebuild it; delete the files and the knowledge is gone.
2. **The index is a cache.** SQLite with full-text and vector search makes retrieval fast. It is never authoritative.
3. **The grammar is the API.** Humans and agents write the same structure — an entity, atomic observations, typed relations — so one format serves both.

If a feature would violate one of these, it does not belong here.

## What you get

- A vault skeleton with an inbox, projects, areas, resources, archive, and meta folders
- `AGENTS.md` — canonical operating rules for any MCP client
- `CLAUDE.md` — imports `AGENTS.md` and adds Claude Code specifics
- `90-Meta/schema.md` — the note grammar, with examples
- Note templates for entities and projects
- Ten golden questions for measuring whether retrieval actually works
- Five project-scoped commands: `/brain-recall`, `/brain-capture`, `/brain-triage`, `/brain-review`, `/brain-backup`
- `docs/architecture.md` — how the pieces fit and where the trust boundaries are

## Prerequisites

| Tool | Version used | Notes |
|---|---|---|
| [Basic Memory](https://github.com/basicmachines-co/basic-memory) | 1.0.0 | Installed with `uv` or `pipx` |
| [Obsidian](https://obsidian.md) | 1.13+ | Desktop; Free |
| [Claude Code](https://claude.com/claude-code) | current | CLI, desktop, or IDE |
| Git | any recent | Backup and versioning |
| Python | 3.12+ | Basic Memory runtime; `uv` manages it |

## Setup

### 1. Install Basic Memory

```bash
uv tool install basic-memory
basic-memory --version
```

If you do not have `uv`:

```bash
# Windows
winget install astral-sh.uv

# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
```

`pipx install basic-memory` works too. If Claude is running as a GUI app and cannot find the binary later, note the absolute path from `where basic-memory` (Windows) or `which basic-memory`.

### 2. Create the vault

Copy this folder to where you want the vault to live, then open a terminal in it.

```bash
# macOS / Linux
cp -R second-brain-starter ~/brain
cd ~/brain
git init

# Windows (PowerShell)
Copy-Item -Recurse second-brain-starter "$env:USERPROFILE\brain"
Set-Location "$env:USERPROFILE\brain"
git init
```

Use a private location. Do not put the vault inside another git repository.

### 3. Register the project and make it the default

```bash
basic-memory project add brain ~/brain --default
basic-memory status
```

On Windows:

```powershell
basic-memory project add brain "$env:USERPROFILE\brain" --default
```

`project add` indexes existing files before returning, so the notes are searchable immediately. If you ever need to reindex:

```bash
basic-memory project index brain
```

### 4. Connect Claude Code

```bash
claude mcp add basic-memory -s user -- basic-memory mcp
```

Restart Claude Code. At user scope, every Claude Code session on the machine can use the vault — including sessions opened in unrelated folders. If that is too broad, omit `-s user` to register it only for the current project.

For Claude Desktop or another GUI client, add the server to its MCP config and use the **absolute path** to the binary if the client cannot find it on `PATH`.

### 5. Install the Claude Code plugin (recommended)

```bash
basic-memory install claude-code --yes
```

This installs the Basic Memory plugin, writes `.claude/settings.json` (primary project, capture and remember folders, session behaviour), and generates note schemas under `schemas/`. Restart Claude Code in the vault afterwards.

The five commands in `.claude/commands/` work without the plugin, but the plugin adds session capture and schema tooling.

### 6. Open the vault in Obsidian

Obsidian → **Open folder as vault** → choose the vault directory. The `.obsidian/` folder will be created on first launch.

### 7. Verify

```bash
basic-memory doctor
```

Then, in a Claude Code session whose working directory is the vault:

> Write a note titled "Setup Test" with one `[fact]` observation, then search for it.

Expected: the file appears in `00-Inbox/Agent/`, and the search returns it. Move the test note to `40-Archive/` when done.

## Connect other agents

Basic Memory is an MCP server, so any MCP-capable client can use the same vault. Each client launches its own server process against the same files; conversations never sync, but a write from one is searchable by the others within seconds.

| Client | Configuration | Rules file |
|---|---|---|
| Claude Desktop | `mcpServers` in `claude_desktop_config.json` | none |
| Codex CLI | `basic-memory install codex`, or `~/.codex/config.toml` | `AGENTS.md` |
| Gemini CLI | `mcpServers` in `~/.gemini/settings.json` | `GEMINI.md` |
| OpenCode | `mcp` in `~/.config/opencode/opencode.json` | `AGENTS.md` |
| Cursor | `mcpServers` in `~/.cursor/mcp.json` | `.cursor/rules/` |
| VS Code (Copilot) | `servers` in `.vscode/mcp.json` | `.github/copilot-instructions.md` |

Always pin the project so a client cannot reach another vault:

```text
command: basic-memory
args: ["mcp", "--project", "brain"]
```

GUI clients often do not inherit `PATH`. If the server fails to start, replace `basic-memory` with the absolute path from `where basic-memory` (Windows) or `which basic-memory`.

### Codex CLI

```bash
basic-memory install codex
```

Manual alternative in `~/.codex/config.toml`:

```toml
[mcp_servers.basic-memory]
command = "basic-memory"
args = ["mcp", "--project", "brain"]
```

Codex reads `AGENTS.md` from the repository root.

### Gemini CLI

`~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "basic-memory": {
      "command": "basic-memory",
      "args": ["mcp", "--project", "brain"]
    }
  }
}
```

Keep `GEMINI.md` as a pointer to `AGENTS.md`.

### OpenCode

`~/.config/opencode/opencode.json`, or `opencode.json` in the vault:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {
    "basic-memory": {
      "type": "local",
      "command": ["basic-memory", "mcp", "--project", "brain"],
      "enabled": true
    }
  }
}
```

OpenCode reads `AGENTS.md`.

### Claude Desktop, Cursor, VS Code

Use the same server block in each client's config:

```json
{
  "mcpServers": {
    "basic-memory": {
      "command": "C:\\Users\\you\\.local\\bin\\basic-memory.exe",
      "args": ["mcp", "--project", "brain"]
    }
  }
}
```

Claude Desktop reads `claude_desktop_config.json`. Cursor accepts the same block in `~/.cursor/mcp.json`. VS Code names the top-level key `servers` and adds `"type": "stdio"` to the entry. A Windows path inside JSON needs doubled backslashes.

Client config formats change; if a key is rejected, check that client's MCP documentation.

### Keep one set of rules

- `AGENTS.md` is canonical. `CLAUDE.md` imports it; `GEMINI.md`, `.cursor/rules/`, and Copilot instructions should point to it rather than duplicate it.
- The `/brain-*` commands work only in Claude Code. Other clients follow the same rules in prose.
- Let one agent own writeback. If several write, give each its own capture folder: `00-Inbox/Agent/Claude/`, `00-Inbox/Agent/Codex/`.
- Deletion stays disabled for every client.

## Vault layout

```text
brain/
├── CLAUDE.md                 agent operating manual
├── 00-Inbox/
│   └── Agent/                default write target for the agent
├── 10-Projects/              active outcomes
├── 20-Areas/                 ongoing responsibilities
├── 30-Resources/             topics, sources, people
├── 40-Archive/               inactive material; never delete
├── 90-Meta/
│   ├── schema.md             the note grammar
│   ├── Golden Questions.md   retrieval evaluation set
│   └── templates/
├── schemas/                  generated by the Claude Code plugin
└── .claude/
    ├── settings.json         plugin configuration
    └── commands/             five vault commands
```

Folders are for human navigation and lifecycle. Retrieval ignores them. What makes a note findable is its title, its observations, and its links.

## Note grammar

```markdown
---
title: Second Brain Build
type: project
tags: [meta, pkm]
---

## Status
- [fact] State: active.
- [fact] Setup assembled 2026-11-25.

## Observations
- [decision] Stack: Basic Memory + Obsidian + Claude over MCP.
- [preference] One idea per note; atomic, categorized facts.
- [todo] Seed 25 real notes.

## Relations
- uses [[Basic Memory]]
- relates_to [[Second Brain Format]]
```

Rules:

- One idea per note, with a claim-shaped title.
- Three to six atomic observations, each categorized: `[fact]`, `[decision]`, `[preference]`, `[todo]`, `[resource]`, `[question]`.
- At least two typed relations: `relates_to`, `requires`, `supersedes`, `owned_by`, `affects`.
- Decisions record their **why**, not just their outcome.
- Every new note links to at least one existing note.

## Commands

These live in `.claude/commands/` and appear in Claude Code's `/` menu when the session starts in the vault.

| Command | What it does | Writes? |
|---|---|---|
| `/brain-recall <question>` | Hybrid search, read top hits, follow relations, answer with `[[citations]]` | No |
| `/brain-capture <text>` | Shapes a thought into a linked, well-formed note | New note in `00-Inbox/Agent/` |
| `/brain-triage [focus]` | Proposes entity notes from inbox material; writes after approval | After approval |
| `/brain-review [project]` | Finds orphans, duplicates, stale hubs; proposes exact edits | After approval; never deletes |
| `/brain-backup [message]` | Stages, commits, and pushes to a private remote | git commit + push |

`$ARGUMENTS` receives everything after the command name, so `/brain-recall why did I pick this stack` passes the whole question.

## Daily loop

| Loop | Cadence | Action |
|---|---|---|
| Capture | anytime | Drop anything into `00-Inbox/` |
| Triage | daily, 10 min | Set type/tags, promote, link |
| Retrieve | every session | Search → context → read → cite |
| Writeback | end of work | Append `[decision]`/`[fact]`/`[todo]` to the project hub |
| Review | weekly | Orphans, duplicates, stale projects, archive |
| Backup | automatic | Commit and push to a private remote |

The loop only compounds if writeback happens. Capture without triage becomes a landfill; triage without writeback never pays off.

## Safety

- **Deletion is disabled by default.** Keep it that way. Move old notes to `40-Archive/`; do not enable `bm_delete` for convenience.
- **Writes are guarded.** New notes are created, not overwritten; edits show a diff first.
- **User-scope MCP is broad.** Every Claude Code session on the machine can read and write the vault. Use project scope if that is unacceptable.
- **The index is local.** Embeddings are computed on-device with FastEmbed; no note text is sent to an embedding API.
- **Back up to a private repository only.** A second brain contains personal context. Never push it to a public repo.
- **The vault is not encrypted.** Do not store credentials, tokens, or other secrets in notes.

## Automatic backup

1. Install the **Obsidian Git** community plugin.
2. Create a **private** remote repository and add it as `origin`.
3. In the plugin settings: commit-and-sync interval `10` minutes, push on sync, pull-before-push on, pull-on-startup off.
4. Run **Obsidian Git: Commit-and-sync** once to establish the baseline.

The timer only runs while Obsidian is open. For coverage outside the app, add a scheduled task that runs the same `git add` / `commit` / `push`.

This starter's `.gitignore` excludes `.obsidian/plugins/` so published templates stay free of third-party binaries. If you want your own vault to back up plugin configuration, remove that line.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Notes land in the wrong project | Run `basic-memory project default brain`. `config set default_project` does not persist in 1.0.0. |
| GUI client cannot find the server | Use the absolute path to `basic-memory` in the MCP config. |
| New notes are not searchable | `basic-memory project index brain`; confirm the file is inside the registered path. |
| Plugin commands do not appear | Restart Claude Code with the vault as the working directory. |
| Reranker is not used | Optional cross-encoder reranking is configured in `~/.basic-memory/config.json`; see the Basic Memory docs for the current key. The model downloads on first use. |
| Index behaves oddly | `basic-memory doctor` runs a sandboxed write/index/search self-test. |

Logs live in `~/.basic-memory/`.

## License

The template files in this repository are MIT licensed — see `LICENSE`. Your notes are yours and are not covered by that license. Basic Memory (AGPL-3.0) and Obsidian Git (GPL-3.0) are installed separately as dependencies, not bundled here; review their licenses before redistributing them or modified versions.
