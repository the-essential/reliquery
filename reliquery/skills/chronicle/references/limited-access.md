# Limited-Access Fallbacks

Guidance for Chronicle sessions without full tool access.

---

## No file system access

- Skip vault gap analysis (Phase 3) — work from memory only
- Generate relics as markdown text in the conversation rather than staging to disk
- Ask the user to manually save the output to their vault, or offer to package relics for download as a zip

---

## No mempalace access

- Skip the memory scan (Phase 1c/1d) — work from the user's provided content and any conversation context only
- Note clearly that the output hasn't been cross-referenced against existing lore and may contain duplicates or contradictions
- Recommend the user run `/memorize` (Audit mode) when back at desktop to reconcile

---

## Google Drive or cloud vault

- If the vault is organized in Google Drive or another cloud folder instead of a local Obsidian vault, Chronicle's workflow is identical — the folder structure and relic format don't depend on Obsidian
- Generate relics as downloadable files or copy-pasteable text and let the user place them in the appropriate Drive folder
- Note that mempalace mining will need to target the local sync folder if using Drive with a desktop sync client

---

The core principle: Chronicle can always produce structured relic content, even when it can't interact with the vault directly. The content is the value; the filing can happen later.
