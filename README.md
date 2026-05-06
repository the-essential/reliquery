# Reliquery

**Persistent AI memory for writers who build worlds.**

Reliquery is a context management and memory system that gives Claude persistent, accurate recall of your creative work across sessions. Instead of re-explaining your characters, factions, and storylines every time you open a new conversation, Reliquery lets Claude *remember* — pulling exactly the lore it needs from a searchable vault of your accumulated worldbuilding.

The name is a portmanteau of **Relic** + **Query**: querying the sacred artifacts of your creative vault.

---

## The Problem

Large language models are stateless. Every conversation starts from zero. If you're building a world with dozens of characters, layered factions, and evolving storylines, you've felt this: the constant re-briefing, the contradictions that creep in when Claude forgets a detail you established three sessions ago, the slow erosion of narrative coherence.

Some workarounds exist — pasting lore into system prompts, maintaining a master document, manually copying context. They all hit the same wall: context windows are finite, and your world isn't. You end up choosing between dumping everything (too much noise) or curating by hand every session (too much work).

## The Solution

Reliquery replaces manual context management with **reconstructive recall** — the same way human memory works. Claude doesn't load your entire world into context. It formulates intelligent questions ("What is Sera's relationship with the Iron Council?"), searches your vault semantically, retrieves the specific passages it needs, and assembles a coherent picture shaped by the current scene.

Your world lives in structured markdown files called **relics** — one per character, location, faction, or concept. The relics are indexed into a local semantic search engine ([MemPalace](https://github.com/MemPalace/mempalace)), and Claude queries that engine through a set of orchestration skills that handle the entire lifecycle: intake, indexing, relationship mapping, and maintenance.

---

## What You Get

### Five Skills, One Pipeline

| Skill | What It Does |
|-------|-------------|
| **Chronicle** | Processes raw content (manuscripts, session logs, brain dumps) into structured vault relics. Handles extraction, staging, review, backup, and commit. |
| **Memorize** | Indexes committed relics into searchable memory. Section-level chunking, duplicate detection, verification. |
| **Cartograph** | Maps entity relationships into a temporal knowledge graph. Tracks who's allied with whom, and when things changed. |
| **Forget** | Controlled deletion of stale or incorrect memory entries. Irreversible, so it always asks first. |
| **Study** | The one-command pipeline: content → relics → memory → knowledge graph. Say "study this" and the system handles the rest. |

Plus **Reliquery Help** as a built-in orientation guide.

### How It Works in Practice

1. You write a chapter, run a co-writing session, or dump notes into a conversation
2. Say "study this" — Reliquery extracts entities, creates structured relics, indexes them into memory, and maps relationships
3. Next session, Claude automatically searches your vault when you mention a character or concept, pulling exactly the context it needs
4. Your world stays consistent across dozens or hundreds of sessions

### What a Relic Looks Like

```markdown
---
type: character
status: active
keywords:
  - Sera
  - Iron Council
  - operative
faction: The Iron Council
relationships:
  - Aldric Thane (rival)
  - Miren Cade (handler)
last-updated: 2026-05-05
---

## Description

Sera moves through crowded rooms the way a scalpel moves
through tissue — with economy and intent.

## Background

Recruited into the Iron Council at seventeen after the
Thorngate Incident left her without family or illusions...

## Notes

Sera's recruitment story mirrors the Council's institutional
philosophy — find people the world has already broken, offer
them purpose, and bind their loyalty through gratitude...
```

Relics are just markdown files with YAML frontmatter. You can edit them in any text editor. [Obsidian](https://obsidian.md) is recommended for its wiki-links and graph view, but it's not required.

---

## Architecture

Reliquery has two layers and a skill orchestration system.

**Layer 1 — The Vault**: A folder of markdown relic files, one per entity. YAML frontmatter provides structured metadata (type, status, keywords, relationships). The vault is the source of truth for all authored content.

**Layer 2 — The Palace**: [MemPalace](https://github.com/MemPalace/mempalace), a local-first AI memory system built on ChromaDB (semantic vector search) and SQLite (temporal knowledge graph). It stores verbatim chunks of your vault content as searchable "drawers," organized into a spatial hierarchy: Wings → Rooms → Drawers.

**Orchestration — Skills**: Chronicle, Memorize, Cartograph, Forget, and Study are Claude skills — repeatable workflow instructions that Claude follows when invoked. No custom Python modules, no server forks, no code to maintain. The skills are portable markdown files.

```
Your Content → [Chronicle] → Vault Relics → [Memorize] → Searchable Memory
                                                              ↓
                                              [Cartograph] → Knowledge Graph
```

### Why Semantic Search, Not Keyword Injection

Early versions of this system used keyword-triggered injection (inspired by SillyTavern's World Info). Testing proved it unnecessary. Claude doesn't search for raw terms — it formulates contextual questions and retrieves relevant fragments from multiple relics, reconstructing coherent context shaped by what the current scene needs. Semantic search is contextual where keyword matching is mechanical.

---

## Installation

### Prerequisites

- Python 3.9+
- [Claude Desktop](https://claude.ai/download) or [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- A Claude Pro, Team, or Enterprise subscription

### Step 1 — Install MemPalace

```bash
pip install mempalace
```

### Step 2 — Connect to Claude

**Claude Code (recommended):**
```bash
claude mcp add mempalace -- python -m mempalace.mcp_server
```

**Claude Desktop:**

Edit your config file:
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "mempalace": {
      "command": "python",
      "args": ["-m", "mempalace.mcp_server"]
    }
  }
}
```

Restart Claude Desktop completely (exit from the system tray, then relaunch).

### Step 3 — Install the Reliquery Plugin

**Claude Code:**
```bash
claude plugin install path/to/reliquery
```

**Claude Desktop / Cowork:**

Install the plugin from the `reliquery/` directory in this repo. The plugin includes all six skills and the MemPalace MCP server configuration.

### Step 4 — Initialize Your Vault

```bash
mempalace init path/to/your/vault
```

The directory name becomes your wing name in the palace. Choose it deliberately.

### Step 5 — Mine Your Content

Mine each content folder separately for clean organization:

```bash
mempalace mine "path/to/vault/World/Characters" --wing your-wing
mempalace mine "path/to/vault/World/Locations" --wing your-wing
mempalace mine "path/to/vault/World/Factions" --wing your-wing
```

Repeat for each content folder. Skip `.obsidian/` and `Templates/`.

### Step 6 — Verify

Ask Claude to call `mempalace_status`. You should see your wing and drawer counts. Test a search against any entity from your vault.

---

## Usage

### Quick Reference

| I want to... | Say this |
|---|---|
| Process raw content into relics | "chronicle this" |
| Index relics into memory | "memorize these relics" |
| Map entity relationships | "cartograph" |
| Remove stale entries | "forget [entity]" |
| Full pipeline in one pass | "study this" |
| Get help | "reliquery help" |

### The Memory Protocol

Every session, Claude follows this sequence automatically:

1. Load the palace layout via `mempalace_status`
2. Identify all entities in your message
3. Search for each entity's prose context and structured state
4. Reconcile any conflicts (knowledge graph wins on current facts; prose wins on voice and detail)
5. Write naturally — never referencing the lookups

When the story changes an entity's state, Claude notes the changes in a changelog block for your review before anything touches the vault.

---

## Vault Structure

```
Your-Vault/
├── World/
│   ├── Characters/         (one relic per character)
│   ├── Locations/          (one relic per place)
│   ├── Factions/           (one relic per organization)
│   ├── Systems-Concepts/   (mechanics, institutions, rules)
│   ├── Magic/              (supernatural systems)
│   ├── Tech/               (technology and devices)
│   └── History/            (past events, foundational lore)
├── Arcs/                   (active storylines, campaign state)
├── Sessions/               (session logs and recaps)
└── Templates/              (relic templates)
```

See the `examples/vault/` directory for sample relics.

---

## Not Just for Fiction

Reliquery was built for worldbuilding and co-writing, but the architecture is domain-agnostic. The same vault-and-memory pattern works for:

- **Tabletop RPG campaigns** — track NPCs, quests, session history, and party state
- **Game design documentation** — mechanics, balance notes, playtest results
- **Research projects** — literature notes, methodology records, evolving findings
- **Any project where an AI collaborator needs persistent, accurate context**

Chronicle's relic format adapts to the use case. Worldbuilding relics have character descriptions and faction structures. Research relics might have methodology sections and open questions. The frontmatter schema flexes; the pipeline stays the same.

---

## Tech Stack

| Component | Tool | License |
|-----------|------|---------|
| Vault / authoring | Markdown files (any editor; [Obsidian](https://obsidian.md) recommended) | — |
| Semantic search | [MemPalace](https://github.com/MemPalace/mempalace) + ChromaDB | MIT |
| Knowledge graph | MemPalace KG (SQLite) | MIT |
| MCP bridge | MemPalace MCP server | MIT |
| Skill orchestration | Reliquery skills (this repo) | MIT |
| AI interface | Claude Desktop or Claude Code | Anthropic |

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines. Bug reports, skill improvements, documentation, and example vault content are all welcome.

## License

[MIT](LICENSE)
