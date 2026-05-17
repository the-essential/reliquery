# Contributing to Reliquery

Hi. I'm Jon — I built this for my own creative writing workflow and released it because the people I showed it to kept asking if they could use it too. If you're reading this, you're probably one of those people. Thanks for being here.

Reliquery is actively maintained by one person (me), which means contributions are genuinely useful — not symbolic. It also means I'd rather get a small, focused PR I can merge in an evening than a sprawling refactor I have to negotiate over a week. Calibrate accordingly.

## What I'd love help with right now

In rough order of how grateful I'd be:

1. **A working Claude Code install path.** Right now `claude plugin install path/to/reliquery.plugin` errors with "not found in any configured marketplace." If you know the actual correct invocation — or have a clean workaround that doesn't require publishing to a marketplace — please open an issue or PR. This is the biggest friction point for new users.
2. **Example relics for use cases that aren't mine.** The `examples/vault/` directory has fiction. `examples/vault-ttrpg/` has a tiny tabletop sample. If you use Reliquery for research notes, game design docs, a D&D campaign you actually run, or anything else — a small set of real-ish relics from your domain would help the next person see themselves in the project. Sanitize as needed.
3. **Troubleshooting documentation.** If you hit a wall during install or first use and figured out the answer, write it down. Even a paragraph in a GitHub issue I can fold into a TROUBLESHOOTING.md is gold.
4. **Skill edge cases.** If Chronicle mis-chunks a weird input, or Memorize creates duplicate drawers under conditions I haven't seen, open an issue with a reproducible example. I can usually fix the skill in a session if I can reproduce the problem.

## What I'd rather you didn't do (yet)

- **Don't rewrite a skill's voice or identity section without talking to me first.** The skill prompts are calibrated — sometimes painfully — to produce specific Claude behaviors. A "cleanup" PR that flattens the voice will usually break something subtle.
- **Don't add dependencies beyond MemPalace and standard Claude features.** The portability of the skills is the whole point. If a problem needs a new dependency, the right answer is usually that the problem belongs upstream in MemPalace or downstream in your own fork.
- **Don't remove the human-in-the-loop checkpoints.** Chronicle and Forget both pause for confirmation in specific places. Those gates exist because I've watched the alternatives go wrong. If you think a checkpoint is unnecessary, open an issue and let's talk before you cut it.

## How to actually contribute

For bug reports, open an issue with:
- What you were trying to do
- What happened instead
- Your setup (Claude Desktop vs. Claude Code, OS, MemPalace version, plugin install method)

For changes:

1. Fork the repo
2. Branch off `main` (`git checkout -b your-change`)
3. Make the change
4. If you touched a skill, run it through a real Claude session on a small piece of content before opening the PR. The skills are too long to fully cover with unit tests; the best validation is "did Claude do the thing"
5. Commit with a message that says *what* changed and *why* (the why matters more than the what)
6. Open a PR. I read them.

## Quick orientation if you're new to the codebase

- `reliquery/skills/` — the six skill files. Each is a single `SKILL.md` with YAML frontmatter and imperative-voice phased instructions. Read end-to-end before editing; the phases reference each other.
- `reliquery/skills/chronicle/references/` — long-form reference material extracted from `SKILL.md` to keep the main file lean. Chronicle loads these on demand.
- `examples/vault/` and `examples/vault-ttrpg/` — sample relics. Safe to use as templates.
- `reliquery.plugin` — the packaged plugin file for Claude Desktop / Cowork.

## Code of conduct

Be kind. Be specific. Assume the other person is doing their best with the information they have. The people using this system are trusting it with their creative work, which is not a small thing — bring that energy to issues and PRs.

If you're not sure whether something is worth opening an issue for, open it. I'd rather close a friendly one than miss a real one.

— Jon
