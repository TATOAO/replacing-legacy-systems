# replacing-legacy-systems

An [Agent Skill](https://agentskills.io/specification) for replacing a running system: rebuilds, rewrites, "upgrades" and migrations of systems that have live data and real users.

[中文说明](README.zh-CN.md)

## Why

Now that AI agents write most of the code, the build itself is no longer the slow part. Rebuilds stall on two other things instead:

- **Facts about the old system that surface late.** For example, a table turns out to start in 2021, or a number the team relies on was never stored anywhere.
- **Decisions that get reopened**, because nobody wrote down what they rested on.

Most existing migration skills cover code translation, the strangler fig pattern and staged rollouts. Base models already know those. This skill covers what they leave out.

## The model

From the old system, take three things:

| Take | What it is |
|---|---|
| **Purpose** | What users actually do with it. This is what makes it this kind of system. |
| **Case history** | Problems it met and never solved. Recurring manual fixes are unwritten bug reports. |
| **Raw materials** | Data and feeds that can't be regenerated. Accept them by measuring them, not by reading their description. |

Leave the **floor plan** behind: the old schemas, job layout, service split and formulas. Use them as evidence, not as the spec.

From those three, write a checkable **standard**. Then build the new system from zero against it, block by block. Each block either **mimics** the old system, in which case the old system is the oracle, or is a **redesign**, in which case you name a new oracle before you start building.

Everything goes in one **ledger** (`ledger-template.md`). Each row records what was measured, the premise the decision rests on, and the observation that would reopen it. When a question surfaces late, only the affected row is reopened.

## Install

Copy `skills/replacing-legacy-systems/` into your agent's skills directory:

- Claude Code: `~/.claude/skills/` (personal) or `.claude/skills/` (project)
- Codex, Copilot CLI or Gemini CLI: `~/.agents/skills/`

## Evidence

`tests/` contains the scenarios used to develop the skill, with answers from agents running without it (baseline) and with it. Baseline agents already handled parallel runs and incident-driven tests well. The skill added the following:

- a measured trust window for each data source
- requirements that trace back to a real use or problem
- a premise and a falsifier recorded for each decision
- a named oracle for each redesigned block

See `tests/results.md`.

The skill's author scored these results, with one run per scenario. Independent blind grading has not been done yet.

## Status

The skill is an early draft, distilled from one real migration. Issues and counter-examples are welcome, especially cases where following the skill made things worse.

## License

MIT
