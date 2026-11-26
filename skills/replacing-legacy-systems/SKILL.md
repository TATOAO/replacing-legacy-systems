---
name: replacing-legacy-systems
description: Use when replacing, rebuilding, rewriting or "upgrading" a running system that holds live data or has active users; when deciding between extending the old system and starting fresh; when planning what to carry over, a data migration or a cutover; or when a rebuild keeps stalling on late discoveries about the old data, reopened decisions, or scope creep.
---

# Replacing Legacy Systems

## Overview

Writing code is cheap now. Knowing what to build, and proving it is right, is not. Most of the time lost in a replacement goes to facts about the old system found late, and to decisions reopened because nobody wrote down what they rested on.

From the old system, carry three things: its **purpose**, its **case history** and its **raw materials**. Leave its **floor plan** behind. Write a checkable **standard** for the new system from those three, then let the new system be built from zero against it.

## The three things you carry

| Carry | What it is | Where it lives | How to collect it |
|---|---|---|---|
| **Purpose** | What users actually do with the system: the contract that makes it this kind of system | Usage logs, screens, reports, downstream readers, people | Observe real use. List every reader of every output, including exports, jobs and spreadsheets. Unused features are not purpose. |
| **Case history** | Problems met and never solved; problems the old design can't avoid | People's heads, tickets, manual fixes, workarounds | Every recurring manual fix is an unwritten bug report. Ask what was tried and abandoned. |
| **Raw materials** | Data and feeds the new system can't regenerate | Tables, streams, files, a clerk's spreadsheet | Accept them by measurement (next section), never by description. |

The **floor plan** is the old system's schemas, job layout, service split and formulas. It is evidence about the three things above. It is not the spec. Modelling new tables on old ones "to save time" inherits the old system's gaps along with its shape.

## Accept raw materials by measuring them

For each material, measure and record: the first and last good date, gaps, granularity, every writer, known defects, and whether it can be collected again. Run the query and keep it.

- A clean first row that looks like a cutover (everything starts on the same date) means other tables probably start there too. Check all of them.
- The trust window limits what the new system can promise. Measure it **before** writing the standard. A report specced "from 2019" over data that starts in 2021 is a spec bug.

## The ledger (required artifact)

Keep one ledger file, using `ledger-template.md`. Every old-system item gets a row before any code that depends on it is written. Required columns:

- **Kind**: purpose / case / material / floor plan
- **Verdict**: carry / redo / transitional (with its removal condition) / unknown (with how and when to find out)
- **Evidence**: what was measured or observed, how, and when
- **Premise**: what must stay true for the verdict to hold
- **Falsifier**: the observation that would reopen it
- **Depends on it**: other rows and blocks

A verdict whose evidence cell is empty counts as **unknown**. An unknown blocks only the code that depends on it.

## Write the standard before building

The standard is a list of checkable statements: invariants, acceptance cases, golden outputs. Each line cites the purpose or case-history row it serves.

- A line that cites neither is a wish, the second-system effect. Park it in a "later" list with its reason.
- Structural choices that are costly to change later get decided now, even without a current user: identity keys, time semantics, the entity model. Record their premise in the ledger.

## Per block: mimic or redesign

Split the work into blocks and pick one mode for each. To choose, ask: when old and new disagree, who is right? If the old system is right by definition (figures already audited, numbers users have tuned against), mimic. If neither is trusted, redesign.

- **Mimic**: the output must match the old system. The old system is the oracle, so parallel-run and diff.
- **Redesign**: the old output is not the target. The old system can no longer judge the result, so name the replacement oracle **before** building. Options: a hand-checked answer table, invariants, an independent recomputation, held-out days or cases.

Diffing a redesigned block against the old one produces only noise. Redesigning without a named oracle means the result can't be verified.

## While you build

- **The old system keeps changing.** Diff its schema, config and new kinds of records on a schedule, and add whatever is new to the ledger.
- **A question surfaces late.** Find its ledger row. If a premise has been falsified, reopen that row and the rows that depend on it, and nothing else. If there is no row, the inventory missed a whole kind of thing: add that kind to the day-one questions.
- **Cutover.** Exactly one writer per piece of data at any time. Per block: parallel-run until a stated number of clean days, move consumers one at a time, keep a switch-back. Delete transitional pieces when their removal condition fires.

## Day-one questions for the humans

1. Which outputs does someone actually read, and what do they do with them?
2. What do you fix by hand, and how often?
3. What did you try to fix and give up on?
4. For each dataset: since when is it complete, who else writes it, and can it be collected again?
5. What must the new system do that the old one never could? Which of those is expensive to add later?
6. During the transition, who writes each piece of data?

## Common mistakes

| Mistake | Fix |
|---|---|
| New schema modelled on old tables | Derive the schema from the standard. Use the old tables only as evidence. |
| "We have data since 2018" taken as fact | Measure the trust window and record the query |
| Scope added "for the future" | It needs a case-history row, or it goes on the later list |
| Every block diffed against the old system | Diff only mimic blocks. Redesigned blocks need a named oracle. |
| A whole design reopened because one assumption failed | Reopen only the row whose premise failed, plus its dependents |
| The inventory treated as a one-time snapshot | Diff the old system on a schedule |

## When not to use

- Framework or version bumps with no change in behavior
- Rewriting a stateless library with no data and no users
