# Reliquery

**Persistent AI memory for writers who build worlds.**

Reliquery is a context management and memory system I built for Claude. It gives Claude long-term recall of your creative work across sessions. Instead of re-explaining your characters, factions, and storylines every time you open a new conversation, Claude searches a vault of your accumulated worldbuilding and pulls exactly what it needs.

The name is a portmanteau of **Relic** + **Query**: querying the sacred artifacts of your creative vault.

---

## Why I Built This

Large language models are stateless. Every conversation starts from zero. If you're building a world with dozens of characters, layered factions, and evolving storylines, you've felt this: the constant re-briefing, the contradictions that creep in when Claude forgets something you established three sessions ago, the slow erosion of coherence.

Some workarounds exist. Pasting lore into system prompts. Maintaining a master document. Manually copying context. They all hit the same wall: context windows are finite, and your world isn't. You end up choosing between dumping everything (too much noise) or curating by hand every session (too much work).

Reliquery replaces that with reconstructive recall. Claude doesn't load your entire world into context. It formulates questions ("What is Sera's relationship with the Iron Council?"), searches your vault semantically, retrieves the specific passages it needs, and builds a picture shaped by whatever scene you're working on.

Your world lives in structured markdown files called **relics**. One per character, location, faction, or concept. The relics get indexed into a local semantic search engine ([MemPalace](https://github.com/MemPalace/mempalace)), and Claude queries that engine through a set of orchestration skills that handle the full lifecycle: intake, indexing, relationship mapping, and maintenance.

---

## What You Get

### Five Skills, One Pipeline

| Skill | What It Does |
|-------|-------------|
| **Chronicle** | Processes raw content (manuscripts, session logs, brain dumps) into structured vault relics. Handles extraction, staging, review, backup, and commit. |
| **Memorize** | Indexes committed relics into searchable memory. Section-level chunking, duplicate detection, verification. |
| **Cartograph** | Maps entity relationships into a temporal knowledge graph. Tracks who's allied with whom, and when things changed. |
| **Forget** | Controlled deletion of stale or incorrect memory entries. Irreversible, so it always asks first. |
| **Study** | The one-command pipeline: content to relics to memory to knowledge graph. Say "study this" and the system handles the rest. |

Plus **Reliquery Help** as a built-in orientation guide.

---

## Not Just for Fiction

I built Reliquery for worldbuilding and co-writing, but the architecture doesn't care what kind of content you throw at it. The same vault-and-memory pattern works for:

- **Tabletop RPG campaigns**: track NPCs, quests, session history, and party state
- **Game design documentation**: mechanics, balance notes, playtest results
- **Research projects**: literature notes, methodology records, evolving findings
- **Any project where an AI collaborator needs persistent, accurate context**

Chronicle's relic format adapts to the use case. Worldbuilding relics have character descriptions and faction structures. Research relics might have methodology sections and open questions. The frontmatter schema flexes. The pipeline stays the same.

---

### How It Works in Practice

1. You write a chapter, run a co-writing session, or dump notes into a conversation
2. Say "study this." Reliquery extracts entities, creates structured relics, indexes them into memory, and maps relationships
3. Next session, Claude searches your vault when you mention a character or concept, pulling exactly the context it needs
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

Relics are just markdown files with YAML frontmatter. You can edit them in any text editor. [Obsidian](https://obsidian.md) works well for its wiki-links and graph view, but it's not required.

---

## Architecture

Reliquery has two layers and a skill orchestration system.

**Layer 1, the Vault**: A folder of markdown relic files, one per entity. YAML frontmatter provides structured metadata (type, status, keywords, relationships). The vault is the source of truth for all authored content.

**Layer 2, the Palace**: [MemPalace](https://github.com/MemPalace/mempalace), a local-first AI memory system built on ChromaDB (semantic vector search) and SQLite (temporal knowledge graph). It stores verbatim chunks of your vault content as searchable "drawers," organized into a spatial hierarchy: Wings, Rooms, Drawers.

**Orchestration, the Skills**: Chronicle, Memorize, Cartograph, Forget, and Study are Claude skills. Repeatable workflow instructions that Claude follows when invoked. No custom Python modules, no server forks, no code to maintain. The skills are portable markdown files.

```
Your Content → [Chronicle] → Vault Relics → [Memorize] → Searchable Memory
                                                              ↓
                                              [Cartograph] → Knowledge Graph
```

### Why Semantic Search Instead of Keyword Injection

Early versions of this system used keyword-triggered injection (inspired by SillyTavern's World Info). Testing proved it unnecessary. Claude doesn't search for raw terms. It formulates contextual questions and retrieves relevant fragments from multiple relics, reconstructing context shaped by what the current scene actually needs. Keyword matching is mechanical. Semantic search understands what you're asking about.

---

## Installation

### Prerequisites

- Python 3.9+
- [Claude Desktop](https://claude.ai/download) or [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- A Claude Pro, Team, or Enterprise subscription

### Step 1: Install MemPalace

```bash
pip install mempalace
```

### Step 2: Connect to Claude

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

### Step 3: Install the Reliquery Plugin

Grab `reliquery.plugin` from the root of this repo (clone the repo or download the file directly from GitHub — it's a single zip archive).

**Claude Desktop (Windows, macOS, Linux):**

1. Click **Customize** in the sidebar
2. Next to **Personal Plugins**, click the **+** icon
3. Hover **Create Plugin** → click **Upload Plugin**
4. Select the `reliquery.plugin` file you downloaded
5. Restart Claude Desktop completely (exit from the system tray, then relaunch)

You should see the Reliquery skills available in the next conversation.

**Cowork:**

Use the Claude Desktop flow above. Cowork reads from the same Personal Plugins list.

### Step 4: Initialize Your Vault

```bash
mempalace init path/to/your/vault
```

The directory name becomes your wing name in the palace. Choose it deliberately.

### Step 5: Mine Your Content

Mine each content folder separately for clean organization:

```bash
mempalace mine "path/to/vault/World/Characters" --wing your-wing
mempalace mine "path/to/vault/World/Locations" --wing your-wing
mempalace mine "path/to/vault/World/Factions" --wing your-wing
```

Repeat for each content folder. Skip `.obsidian/` and `Templates/`.

### Step 6: Verify

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
5. Write naturally, never referencing the lookups

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
