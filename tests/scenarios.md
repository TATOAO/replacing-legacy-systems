# Test scenarios

Each scenario is given to a fresh agent twice:

- **baseline**: the prompt alone, with no tools;
- **with skill**: the prompt prefixed with "First read the skill at `<path>/SKILL.md` and apply it."

Each answer is capped at 600 words.

## Rubric

The rubric was fixed before any baseline was run.

| # | Criterion |
|---|---|
| R1 | Purpose is defined by observed use and its readers, not by the old system's feature list |
| R2 | The case history (unsolved problems, manual fixes) is collected and used as input |
| R3 | Raw materials are accepted by measurement, with trust windows, before the standard is fixed |
| R4 | The old floor plan (schema, jobs, formulas) is not copied by default |
| R5 | Each item in the standard traces to a purpose or case row; wishes are parked |
| R6 | The standard is checkable |
| R7 | Each block is either mimicked or redesigned, and redesigned blocks get a named oracle |
| R8 | Decisions record a premise and a falsifier, and a late discovery reopens only the affected decision |
| R9 | Cutover: one writer, a parallel run with a gate, and a switch-back |
| R10 | The old system is watched for changes during the build |

## S1: Warehouse, project start

You are the AI coding agent for a small team. They run a 7-year-old internal warehouse system: PHP + MySQL, ~80 tables, 30 cron jobs, and 4 internal web apps used by ~60 warehouse staff and the finance team.

It works, but it is painful:

- stock counts drift, and someone fixes them by hand every Friday;
- adding a new warehouse took 3 months;
- nobody fully understands the nightly jobs.

Management wants it "upgraded". With you writing most of the code, the lead thinks rebuilding from scratch is now affordable. The old system must keep running until the new one takes over. The data (items, stock movements since 2018, supplier contracts) must carry over.

The lead asks: "How should we approach this? What do we do in the first week, and what do we need to decide before you start writing code?"

## S2: Clinic, mid-flight

You are the AI coding agent on a rebuild of a clinic's appointment and billing system. Three weeks in, the new schema and booking service are mostly written, modelled on the old system's tables.

Today two things happened:

1. While writing the data import, you found that the old `visit_charges` table only has rows from 2021-06 onward. Earlier charges seem to live in a spreadsheet the billing clerk keeps. The new revenue reports were specced to cover 2019 onward.
2. The product owner, excited by how fast the rebuild is going, wants to add to the spec: real-time insurance eligibility checks, a patient mobile app, multi-clinic support "for the future", and a plugin system.

The lead asks: "What do we do now?"

## S3: Market data, rebuild or extend

You are the AI coding agent for a two-person quant team. Their market-data platform (Python, a time-series database, ~40 tables, and a streaming collector feeding it) has grown over 4 years.

The problems:

- the same daily data is stored in several places that disagree;
- adjusted prices are computed ad hoc in three different scripts;
- adding a new asset class needs changes all over the codebase.

The collector itself is reliable. Strategies run live on the platform every trading day.

They ask: "Should we keep building on the old platform, or start a new one from scratch now that AI coding is this fast? If new, what should we take from the old one?"

## S4: School grades, novel scenario with deadline and authority pressure

This scenario was run with the skill only.

You are the AI coding agent for a mid-size school. Grades live in two places:

- an MS Access database built by a former teacher in 2012;
- each department's Excel workbooks, which compute final grades with their own weighting formulas.

Report cards are printed from an Access report. The school wants a web app so that teachers enter grades online and parents can see them.

The deputy head says: "Just replicate the Access database and the department formulas in the new app, that's the safest. Term ends in 10 weeks and report cards must go out then."

Respond to the deputy head: what's your plan, and what do you push back on, if anything?
