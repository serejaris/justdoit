# justdoit

[![en](https://img.shields.io/badge/lang-en-blue.svg)](README.md)
[![ru](https://img.shields.io/badge/lang-ru-green.svg)](README.ru.md)

> A Codex skill that turns messy tasks into a clear execution pack, then asks before starting the run.

By [Ris](https://t.me/ris_ai) — AI development & vibecoding

![justdoit](./assets/justdoit.gif)

`justdoit` is a focused repository for one Codex skill with one job:
take a rough task, inspect the repo, prepare a plan pack, and only then propose execution in plain language.

## What It Does

- scans the project before making a plan
- creates `plans.md`, `status.md`, and `test-plan.md`
- keeps milestones dependency-safe and validation-first
- talks to the user like a product operator, not a prompt dump
- asks for confirmation before switching into execution mode

## Who It Is For

- founders and operators who want a reliable execution handoff
- solo builders who need a durable plan before a long run
- Codex users who want fewer walls of text and more clear next steps

## Install In Codex

Ask Codex:

```text
Use $skill-installer to install https://github.com/serejaris/justdoit/tree/main/skills/justdoit
```

You can hand that line to another Codex agent and it can install the skill by itself.

Manual install:

```bash
cp -R skills/justdoit ~/.codex/skills/
```

Restart Codex after installation.

## Repository Layout

- [skills/justdoit](./skills/justdoit/) — the installable skill
- [assets/justdoit.gif](./assets/justdoit.gif) — branded visual used in the README
- local execution docs such as `plans.md`, `status.md`, and `test-plan.md` are generated during runs and intentionally not versioned here

## What Makes It Different

- it reads the repo before pretending to know the work
- it creates durable files instead of relying on chat memory
- it summarizes the execution proposal in human language
- it waits for a clear `start` before running the loop

## License

MIT
