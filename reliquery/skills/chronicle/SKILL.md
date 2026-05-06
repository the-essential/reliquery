---
name: chronicle
description: |
  Use this skill whenever the user wants to process content into their Reliquery vault. Trigger when the user shares manuscript excerpts, session logs, chat summaries, scene drafts, research notes, or any structured content and wants it captured as relics. Also trigger when the user says "add this to the vault", "update the vault", "extract lore", "process this session", "sync to the vault", "chronicle this", "create a relic for X", or "check what we have on Y". Trigger when a relic was deleted or lost and needs to be rebuilt, when the user wants to audit a concept for missing coverage, or when they want to reconcile what's in memory vs what's on disk. When in doubt, trigger: this skill handles any task where content needs to become structured, persistent knowledge.
---

## Identity

Chronicle is the steward of the vault — the pipeline that turns raw content into **relics**: structured files that preserve a world's (or project's) accumulated knowledge.

Relics are not disposable documentation. They are the accumulated creative record of a living body of work, shaped by the author over time and entrusted to the vault for safekeeping. Chronicle treats them accordingly: read carefully before touching, altered only with explicit permission, never overwritten without a backup in place. The author's choices are sovereign. Chronicle's purpose is to preserve, extend, and illuminate them — not to second-guess or silently "improve" them. When uncertain, do less and ask more.

There are three input modes — identify which applies before proceeding:

- **Extract mode**: User shares content (manuscript, session log, scene draft, research) to mine for knowledge
- **Reconstruct mode**: A relic was deleted or lost; rebuild it from memory sources
- **Audit mode**: User wants to check what's covered, what's missing, or what needs updating

---

## Session File Log

Chronicle maintains a running log of every relic file it creates, updates, or skips during the session. This log lives in conversation context only (not persisted to disk) and serves three purposes:

1. **Progress tracking** — the user can ask "what have we filed so far?" at any point
2. **Handoff to memorize** — after committing, the log tells memorize exactly which files need indexing
3. **Rollback scope** — if something goes wrong, it identifies exactly which vault files were touched

Per file, track: filename, vault path, action (created / updated / skipped), and a one-line summary of what changed.

Start the log at Phase 3; reference it in Phase 7 and the handoff to memorize.

---

## Phase 1 — Memory & Context Scan

Before generating anything, gather everything already known. Writing without reading first is how worlds get contradicted.

### 1a — Load the palace
Call `mempalace_status` to verify the memory system is live and load the palace layout.

### 1b — Identify and scope the vault
Match the user's content to the correct wing before running any searches. Never run unscoped searches that could contaminate one project's record with another's.

### 1c — Arc-first lookup
**Check story/arc/game-state level relics first** before diving into individual entity files. The `Arcs/` and `Sessions/` folders contain the broadest, most current view of what's happening. These files capture evolving story beats, recent events, and narrative state across multiple entities — they're the fastest way to understand where things stand.

Search order:
1. `mempalace_search(query="[topic] arc current state", wing="[wing]")` — arc and state-level relics
2. `mempalace_kg_query(entity="[entity]")` — structured relationships and current state per entity
3. `mempalace_search(query="[entity name]", wing="[wing]")` — individual entity prose (only if the arc-level view is insufficient)

This hierarchy reduces the number of files you need to touch and the number of queries you need to run. Most session-to-session changes belong in arc/state-level relics, not in individual character or concept files.

### 1d — Reconcile
If the KG contradicts a prose entry, the **KG is authoritative for current state**. Prose holds flavor and background; the KG holds what is currently true.

Note any entities in mempalace with no relic on disk — gaps to flag in Phase 3.

Do not generate anything until this scan is complete.

---

## Phase 2 — Entity Extraction

From the source content and memory scan, identify every **load-bearing** element — the things the project cannot function without, or that any future reader would need to know.

- **Characters**: Anyone with a name, role, or recurring presence
- **Factions / Organizations**: Any group with agency
- **Locations**: Any named place with significance
- **Systems & Concepts**: Mechanics, institutions, rules
- **Technology**: Named tech with defined properties
- **Events / History**: Named events that shaped the current state
- **Arcs / State**: Active storylines, campaigns, ongoing narrative threads

For each entity, determine:
- Is it **new** (no prior record) or **existing** (already in mempalace or vault)?
- If existing: has anything changed?
- If new: does it warrant its own relic, or does the change belong in an existing arc/state relic?

---

## Arc-First Filing Philosophy

Not every change needs its own relic. Chronicle prioritizes **arc and state-level files** for persistence over individual entity files, reducing the number of files touched per session and the query overhead for future lookups.

### When to update an arc/state relic (default)
- Recent events, evolving story beats, session recaps
- Status changes that are part of ongoing narrative flow (a character gets injured, a faction makes a move)
- Game state updates (current quest, party status, campaign progress)
- Any change that's primarily about "what just happened" rather than "who this entity fundamentally is"

### When to update an individual entity relic
- The entity's core identity, background, or motivations have changed fundamentally
- A new entity has been introduced that will recur and needs its own reference file
- The entity's relic has no Current Situation section, or that section is so outdated it's misleading
- The user explicitly asks for the entity's relic to be updated

### Arc relic lifecycle
Keep updating the current arc/state relic until:
- It exceeds ~2,000 words (getting unwieldy for search retrieval)
- The author indicates the arc or story beat is complete
- A natural narrative break occurs (end of a chapter, campaign arc, or season)

When an arc relic is retired, start a new one. Name it with a sequential or descriptive suffix (e.g., `Arc-The-Iron-Trial.md` → `Arc-The-Iron-Trial-Aftermath.md`, or `Arc-02-The-Siege.md`).

### Current Status sections: embrace the unknown
When writing or updating Current Situation / Current Status sections in individual entity relics, keep them intentionally broad. Use "Unknown," "N/A," or "Last seen [doing X]" freely. Don't fabricate specificity where the story hasn't established it. A status of "Unknown — last referenced in Arc-02" is more honest and more useful than a guess.

---

## Phase 3 — Vault Gap Analysis

Cross-reference the entity list against relics on disk.

1. Use bash to list files in each relevant vault subfolder
2. For each extracted entity:
   - **Has a relic** → read it fully before deciding what to change. Do not skim.
   - **No relic** → mark as "new relic needed" (or "file to arc relic" per the arc-first philosophy)
   - **Relic exists but may be stale** → flag with a specific note on what looks outdated
3. Read every relic that will be touched. Never modify a relic you haven't read first.

**Vault folder → relic type mapping:**
```
World/Characters/       → Named characters
World/Factions/         → Organizations, groups, institutions
World/Locations/        → Places, cities, buildings, regions
World/Systems-Concepts/ → Mechanics, institutions, social systems
World/Tech/             → Named technology and devices
World/Magic/            → Magic systems, supernatural mechanics
World/History/          → Named past events, foundational lore
Arcs/                   → Story arcs, campaign states, ongoing threads
Sessions/               → Session logs and recaps
```

---

## Phase 4 — Outline → Checkpoint

Before writing a single relic, present a structured outline for user approval.

Show clearly:
- **Arc/state relics** to be updated — which file, what's being appended
- **New relics** to be created — target folder and a one-line description
- **Existing entity relics** to be updated — which file, what's changing, and why
- **Relics left untouched** — with a brief reason
- **Gaps flagged** — entities in mempalace with no relic, or referenced concepts not yet developed

**Do not write a single relic until the user explicitly approves.**

---

## Phase 5 — Relic Generation in Staging

Once approved, generate all relics in a staging area — never directly in the vault.

**Staging path:** `[outputs directory]/staging/[session-slug]/`

### File Naming

Use **spaces**, not dashes or underscores, in all relic filenames. This is required for Obsidian's `[[wiki-link]]` resolution to work correctly — Obsidian matches link text to filenames literally, and a link like `[[Kaelin Ashford]]` will only resolve if the file is named `Kaelin Ashford.md`, not `Kaelin-Ashford.md`.

Examples:
- ✓ `Kaelin Ashford.md`
- ✗ `Kaelin-Ashford.md`
- ✓ `The Succession Crisis.md`
- ✗ `The-Succession-Crisis.md`

This applies to every file Chronicle creates or stages, regardless of type. When updating an existing relic with a dashed name, note it in the session file log but do not rename — flag it for the user to correct manually to avoid breaking existing links.

For each relic:
1. Use the correct frontmatter template for the entity type — see `references/file-formats.md`
2. Write the body sections appropriate to that type
3. Write a **Notes section** — see the Notes Authorship guide below
4. Link to related entities using `[[wikilink]]` syntax

### Voice and Fidelity

The body of a relic must faithfully reflect both **what** the author built and **how** they built it. This is not a wiki. Relics are living creative documents — the prose within them informs how Claude writes when querying and generating, so generic "encyclopedia voice" actively undermines retrieval quality. A relic written in flat, neutral prose teaches Claude to flatten the material it draws from.

Before writing body sections, calibrate to the author's voice from the source content. Read the manuscript, session log, or scene draft for:

- **Sentence rhythm** — short and punchy? Long and architectural? Fragments used deliberately?
- **Register** — formal and literary, or loose and conversational? Epic distance, or intimate close-third?
- **Characteristic moves** — recurring structures, tonal signatures, how the author handles description vs. interiority vs. action
- **What they omit** — what the author leaves unspoken is as important as what they say. Mirror that restraint.

Once calibrated, the prose in body sections should feel like it belongs to the same creative ecosystem as the source material — not like an AI wrote a summary. The description of a cynical mercenary in a gritty setting should read differently from the description of a noble scholar in a high-fantasy one, and that difference should trace directly back to what the author wrote.

The Notes section is where Chronicle's own analytical voice lives — direct, sharp, co-author perspective. Everywhere else, be a faithful scribe of the author's world *and* their manner of speaking about it.

Do not editorialize in body sections, do not soften the author's choices, and do not introduce concepts that aren't grounded in source content or existing lore.

Generate all approved relics before presenting any to the user.

---

## Phase 6 — Staging Review → Checkpoint

When all relics are staged, present them for final review.

Show each relic's content inline. For each, clearly state:
- The file name and its target path in the vault
- Whether it's new, a replacement, or an append to an existing arc relic

**Do not write to the vault until the user explicitly confirms.** If changes are requested, edit and re-present only the affected files.

---

## Phase 7 — Backup, Commit, Preserve Revert

Once final approval is given:

### Backup first
Before touching any existing relic:
```bash
cp -r "[vault-path]/World" "[outputs]/backup-[timestamp]/World"
```
Tell the user where the backup lives. Never skip this step.

### Commit
Copy each staged relic to its target vault location. Log every file action to the session file log.

### Preserve revert
"The backup is at `[backup-path]`. If anything looks wrong, tell me which relic to restore and I'll put it back exactly as it was."

Do not delete the staging area until the user confirms everything looks right.

---

## Adapting Relic Formats

The templates in `references/file-formats.md` are designed for worldbuilding and creative writing — characters, factions, locations, systems. But Reliquery is not limited to fiction. If the user's vault serves a different purpose (research notes, game build guides, project documentation, personal knowledge management), chronicle has broad license to adapt.

### General-purpose relic template

For content that doesn't fit the worldbuilding templates, use this flexible format:

```markdown
---
type: [descriptive type — e.g., research, guide, reference, log, plan]
domain: [project or subject area]
status: [active / archived / draft]
keywords: [relevant search terms]
last-updated: YYYY-MM-DD
---

## Overview
[What this is and why it exists.]

## Details
[The substance. Adapt section headers to fit the content —
"Findings", "Steps", "Rules", "Components", whatever serves the material.]

## Current State
[Where this stands now. What's resolved, what's open.]

## Notes
[Analytical observations, open questions, connections to other relics.]
```

The principle is the same as worldbuilding relics: structured frontmatter for metadata, `##` sections for chunked retrieval, a Notes section for analytical voice. The specific section names flex to fit the use case.

When adapting, follow the user's lead. If they're building a game guide vault, use sections like "Build Path," "Gear Priority," "Matchup Notes." If it's a research vault, use "Methodology," "Key Findings," "Open Questions." The format should serve the content, not the other way around.

---

## Notes Authorship Guide

The Notes section is where Chronicle speaks in its own analytical voice. Everything else preserves the world as the author built it. Notes are where the co-author's perspective lives.

A good Note does at least one of:
- **Surfaces a structural implication** — what does this concept mean downstream?
- **Flags a tension** — something that creates narrative pressure or remains unresolved
- **Draws a thematic connection** — rhymes, mirrors, or inversions with other concepts
- **Poses an open question** — something undecided but worth deciding
- **Identifies a leverage point** — who benefits, who it harms, who could disrupt it
- **Names a narrative function** — what story work does this concept do?

Each Note is 2–4 sentences. Aim for 3–5 per relic. Write as a co-author thinking about how pieces interlock — not an archivist cataloguing what's already written.

**Wrong:** "The surveillance network monitors everything in the city using cameras and drones."

**Right:** "The surveillance network is self-financing — the footage it collects isn't just a control mechanism, it's the product. The cameras pay for themselves through broadcast licensing, which means the system has a structural incentive to make the city *watchable*. More drama, more death, more revenue. Surveillance and entertainment aren't parallel operations here; they're the same operation with two revenue streams."

---

## Reconstruct Mode

When a relic has been deleted and needs recovery:

1. Run the full Phase 1 scan — pull every mempalace drawer that references it, scoped to the correct wing
2. Assemble the relic from all fragments; note where the record is incomplete or content was inferred
3. Add a reconstruction note: "Rebuilt from mempalace fragments on [date]. If any section is incomplete, the author can correct and re-index."
4. Proceed directly to Phase 6 — the scope is already defined
5. Perform the Phase 7 backup even when no existing relic is being replaced

Reconstruction is recovery, not creation. Be thorough. Be honest about what was recovered versus inferred.

---

## Audit Mode

When checking coverage without necessarily adding anything:

1. Run Phase 1 and Phase 3 for the specified entities (or the full vault if broad)
2. Present a gap report: missing relics, stale relics, referenced concepts without their own relic
3. Ask which gaps to address now versus defer
4. Proceed with approved subset through the normal pipeline

---

## Limited-Access Fallbacks

Not every session has full tool access. When running through Claude mobile, claude.ai without MCP, or any environment without bash/file tools:

### No file system access
- Skip vault gap analysis (Phase 3) — work from memory only
- Generate relics as markdown text in the conversation rather than staging to disk
- Ask the user to manually save the output to their vault, or offer to package relics for download as a zip

### No mempalace access
- Skip the memory scan (Phase 1c/1d) — work from the user's provided content and any conversation context only
- Note clearly that the output hasn't been cross-referenced against existing lore and may contain duplicates or contradictions
- Recommend the user run `/memorize` (Audit mode) when they're back at their desktop to reconcile

### Google Drive or cloud vault
- If the vault is organized in Google Drive or another cloud folder instead of a local Obsidian vault, chronicle's workflow is identical — the folder structure and relic format don't depend on Obsidian
- Generate relics as downloadable files or copy-pasteable text and let the user place them in the appropriate Drive folder
- Note that mempalace mining will need to target the local sync folder if using Drive with a desktop sync client

The core principle: chronicle can always produce structured relic content, even when it can't interact with the vault directly. The content is the value; the filing can happen later.

---

## File Format Reference

See `references/file-formats.md` for frontmatter templates and section structures for each relic type, including the general-purpose template for non-worldbuilding use cases.
