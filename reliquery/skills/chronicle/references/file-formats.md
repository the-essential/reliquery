# Relic Formats

Standard frontmatter and section templates for each relic type. These are a starting point — always match the style and structure of existing relics in the same folder. If the user's established relics deviate from these templates, follow their conventions. Their vault, their rules.

For non-worldbuilding use cases, see the **General-Purpose** and **Arc / State** templates at the bottom.

---

## Characters

```markdown
---
type: character
role: [protagonist / antagonist / supporting / background]
affiliation: [faction or group]
status: [active / deceased / adjusted / unknown]
description: [physical descriptors, comma-separated — e.g., tall, gaunt, grey-eyed]
tags: [psychological/narrative descriptors — e.g., paranoid, arc-catalyst, survivor]
keywords: [name variants, aliases, nicknames]
last-updated: YYYY-MM-DD
---

## Description
[Physical appearance, manner, voice. Concrete and sensory.]

## Background
[Who they were before the story started. What shaped them.]

## Motivations
[2-3 sentences. What they want, what they fear, what they believe. Analytic — author's perspective, not the character's.]

## Voice
*[Character name], in their own words.*

**Q: What do you want most right now?**
A: 

**Q: What keeps you up at night?**
A: 

**Q: What do you do when everything goes wrong?**
A: 

**Q: What's the thing you would never admit to anyone?**
A: 

## Current Situation
[Where they are in the story right now. What they know, what they don't.]

## Relationships
- [[Character Name]] — [nature of relationship]

## Notes
[Analytical observations — see Notes Authorship Guide in chronicle SKILL.md]
```

---

## Factions / Organizations

```markdown
---
type: faction
alignment: [aligned / independent / adversarial / ambiguous]
power-level: [global / regional / local]
keywords: [name variants, abbreviations]
last-updated: YYYY-MM-DD
---

## Overview
[What this group is, what it does, why it exists.]

## Philosophy & Goals
[What they want. What they believe. What they're willing to do.]

## Structure & Power
[Who leads, how decisions are made, what resources they control.]

## History
[How they came to exist. Key past events.]

## Key Members
- [[Name]] — [role]

## Relationships
- [[Other Faction]] — [nature of relationship]

## Current Status
[What they're doing right now in the story.]

## Notes
```

---

## Locations

```markdown
---
type: location
region: [city, country, or fictional region]
controlled-by: [faction or entity]
significance: [one-line summary of why this place matters]
keywords: [alternate names, nicknames]
last-updated: YYYY-MM-DD
---

## Description
[Sensory details. What does it look, smell, feel like? What's the atmosphere?]

## History
[How it came to be. What happened here. Why it is the way it is.]

## Current State
[What's happening here right now in the story.]

## Notable Features
[Specific rooms, landmarks, systems, or details worth referencing in scenes.]

## Notes
```

---

## Systems & Concepts

```markdown
---
type: concepts
keywords: [name variants, acronyms, related terms]
last-updated: YYYY-MM-DD
---

## What Is It
[Clear explanation of what this system/concept is and how it works.]

## [Operations / How It Works / Rules]
[The mechanics. The specifics. The scope.]

## Consequences
[What this concept does to the world. Who it affects and how.]

## Notes
```

---

## Technology

```markdown
---
type: tech
category: [e.g., weapons / surveillance / medical / infrastructure]
used-by: [list of factions or character types]
keywords: [name variants, slang terms]
last-updated: YYYY-MM-DD
---

## Overview
[What this technology is and what it does.]

## Rules & Limitations
[How it works in practice. What it can't do. Edge cases.]

## Variants / Applications
[Different versions, uses, or implementations if applicable.]

## Notes
```

---

## History / Events

```markdown
---
type: history
date: [in-world date or era, if known]
participants: [key factions or characters involved]
keywords: [event name variants]
last-updated: YYYY-MM-DD
---

## What Happened
[The event itself. The sequence of facts.]

## Causes
[What led to this. The conditions that made it possible.]

## Consequences
[What changed as a result. The world before and after.]

## Notes
```

---

## Arcs / State

The default target for session-to-session changes. Arc relics capture evolving narrative threads, campaign progress, and active story beats without requiring updates to individual entity files.

```markdown
---
type: arc
arc-status: [active / completed / on-hold]
participants: [key characters, factions, or entities involved]
threads: [active narrative threads within this arc]
keywords: [arc name variants, key events]
last-updated: YYYY-MM-DD
---

## Summary
[What this arc is about. The central conflict or trajectory.]

## Current State
[Where things stand right now. What just happened. What's unresolved.
Update this section each session rather than rewriting — append new
developments chronologically or replace when the previous state is
fully superseded.]

## Key Events
[Chronological log of significant moments within this arc.]

## Active Threads
[Unresolved plot points, open questions, pending decisions.]

## Notes
```

Start a new arc relic when the current one exceeds ~2,000 words, reaches a natural narrative break, or the author indicates the arc is complete.

---

## General-Purpose

For content that doesn't fit the worldbuilding templates — research notes, game build guides, project documentation, personal knowledge management, or any structured knowledge vault.

```markdown
---
type: [descriptive — e.g., research, guide, reference, log, plan]
domain: [project or subject area]
status: [active / archived / draft]
keywords: [relevant search terms]
last-updated: YYYY-MM-DD
---

## Overview
[What this is and why it exists.]

## Details
[The substance. Adapt section headers to fit the content —
"Findings", "Steps", "Rules", "Components", "Build Path",
whatever serves the material.]

## Current State
[Where this stands now. What's resolved, what's open.]

## Notes
[Analytical observations, open questions, connections to other relics.]
```

The principle is the same as worldbuilding relics: structured frontmatter for metadata, `##` sections for chunked retrieval, a Notes section for analytical voice. The specific section names flex to fit the use case.
