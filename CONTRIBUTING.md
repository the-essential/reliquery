# Contributing to Reliquery

Thanks for your interest in contributing. Reliquery is a personal project that grew out of a real creative writing workflow, and contributions that improve the system for other writers and worldbuilders are welcome.

## How to Contribute

### Reporting Issues

If something breaks, a skill behaves unexpectedly, or you have a feature idea, open a GitHub issue. Include:

- What you were trying to do
- What happened instead
- Your setup (Claude Desktop vs. Claude Code, OS, MemPalace version)

### Suggesting Changes

1. Fork the repository
2. Create a branch for your change (`git checkout -b my-change`)
3. Make your edits
4. Test the affected skills in a Claude session if possible
5. Commit with a clear message describing what changed and why
6. Open a pull request

### What Makes a Good Contribution

- **Skill improvements**: Better chunking logic, smarter duplicate detection, clearer checkpoint prompts, edge case handling
- **Documentation**: Clearer setup instructions, additional examples, troubleshooting tips
- **Example vault content**: Sample relics for different use cases (game design, research notes, campaign logs) that help new users understand the format
- **Bug fixes**: Anything that makes the pipeline more reliable

### What to Avoid

- Don't rewrite a skill's voice or identity section without discussion — these are calibrated to produce specific Claude behaviors
- Don't add dependencies on tools or services beyond MemPalace and standard Claude capabilities
- Don't remove human-in-the-loop checkpoints — the confirmation gates exist for safety

## Skill Architecture

Each skill is a single `SKILL.md` file with YAML frontmatter (name, description) and a markdown body containing phased instructions for Claude. The body is written in imperative voice — it tells Claude what to do, not the user.

If you're modifying a skill, read it end-to-end before changing anything. The phases are interdependent and the ordering is deliberate.

## Code of Conduct

Be kind. Be constructive. Remember that the people using this system are entrusting their creative work to it.
