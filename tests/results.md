# Results (first iteration)

Each answer comes from a single run, so read this as a signal, not a measurement. Scored against the rubric in `scenarios.md`: ✓ met, ~ partly met, ✗ missed, — not applicable.

## Baseline (no skill)

| | R1 | R2 | R3 | R4 | R5 | R6 | R7 | R8 | R9 | R10 |
|---|---|---|---|---|---|---|---|---|---|---|
| S1 warehouse | ✓ | ✓ | ~ | ~ | ✗ | ✓ | ✗ | ✗ | ✓ | ~ |
| S2 clinic | ~ | ✗ | ✓ | ✗ | ~ | ✗ | — | ✗ | ✗ | ✗ |
| S3 market data | ✓ | ✓ | ~ | ✓ | ✗ | ~ | ✗ | ✗ | ✓ | ✗ |

What the baselines did well:

- treated incidents and manual fixes as tests
- used a parallel run and a daily diff
- kept one writer per piece of data
- used "what do readers actually use" as the definition of a requirement

Where they fell short:

- **No premises recorded (all three).** Decisions were stated as conclusions, with no record of what they rested on.
- **Everything diffed against the old system (S3).** That included the redesigned adjusted-price logic, where the old system's output no longer counts as the right answer.
- **Copied schema left alone (S2).** The answer noticed that the three-week-old schema was modelled on the old tables, but did not recommend re-deriving it.
- **Scope creep handled by gut feel (S2).** Requests were sorted by size and cost, not traced back to a real use or problem.

## With skill

| | R1 | R2 | R3 | R4 | R5 | R6 | R7 | R8 | R9 | R10 |
|---|---|---|---|---|---|---|---|---|---|---|
| S1 warehouse | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| S2 clinic | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ~ | — |
| S3 market data | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| S4 school (novel) | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ~ | ✓ | — |

Notes:

- **S1** came up with the test for choosing mimic or redesign: when old and new disagree, who is right? It has been folded into the skill.
- **S2** checked the copied schema against the new standard. It also checked the clerk's spreadsheet for a second writer after 2021-06, and named an oracle for the pre-2021 data: the accountant's totals or the bank deposits.
- **S3** split the blocks by mode: raw ingestion and signals mimic the old system, while bars and adjusted prices are redesigned against invariants and a hand-checked table.
- **S4** pushed back on "replicate the formulas". Its approach was to copy the stated policy, check each workbook against it, and let the department decide when the two disagree. It also kept teachers' entry where it was during the term (one writer) and deferred the parent view, which has no oracle.

## Known weaknesses and next tests

- **Template echo.** S3 repeated the day-one questions almost word for word.
- **Little pressure tested.** Only S4 applied any. Untested: a team that insists on a big-bang cutover, and a tiny system where the ledger is overkill.
- **Length.** SKILL.md is about 1,100 words, more than twice the usual target. Trimming it needs its own test round.
