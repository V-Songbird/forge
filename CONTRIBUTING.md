# Contributing

Forge is maintained by a single author. Contributions are welcome in the form of bug reports, suggestions, and pull requests.

## Before opening a PR

- Check existing issues first — the problem may already be tracked or intentionally deferred.
- For substantial changes (new skills, new agents, significant pipeline changes), open an issue first to align on direction before writing anything.

## Repository structure

```
forge/
├── .claude-plugin/
│   └── plugin.json              # name, description, author, keywords, hooks wiring
├── CHANGELOG.md                 # Keep a Changelog format
├── LICENSE
├── README.md
├── assets/
│   └── logo.svg
├── agents/                      # Subagent definitions (Claude Code frontmatter + instructions)
│   ├── forge-expert.md
│   ├── adversarial-critic.md
│   └── forge-implementer.md
├── hooks/
│   ├── forge-hooks.json         # Hook event wiring
│   ├── forge-runtime.js         # Shared flag-file utilities
│   ├── forge-activate.js        # UserPromptSubmit — level tracking + routing hints
│   └── forge-subagent.js        # SubagentStart — citation-discipline injection
└── skills/
    ├── forge/SKILL.md           # Pipeline orchestrator (entry point)
    ├── expert-analysis/
    ├── master-plan/
    ├── critic-review/
    ├── plan-revise/
    ├── dispatch-implementation/
    └── build-and-report/
```

## What to keep in mind

**Skills and agents are Claude-facing instruction files.** Changes to `SKILL.md` or agent `.md` files affect how Claude interprets the pipeline — be precise, and test manually by running the affected skill in a real session before submitting.

**Hooks are Node.js scripts.** `forge-activate.js` and `forge-subagent.js` run on every prompt and subagent start respectively. Keep them fast (no network, no blocking I/O) and test on both Unix and Windows (`node hooks/forge-activate.js` with a mocked stdin).

**The pipeline is sequential and stateful.** Each skill hands off to the next. Changes to one step's output format (expert reports, master plan structure, critic findings) may break downstream steps — check the full chain.

## Changelog

Add an entry to `CHANGELOG.md` under `[Unreleased]` for every user-visible change. Follow the [Keep a Changelog](https://keepachangelog.com/en/1.1.0/) format. Version bumps happen at release time, not per-PR.

## Code of conduct

This project follows the [Contributor Covenant 2.1](./CODE_OF_CONDUCT.md).
