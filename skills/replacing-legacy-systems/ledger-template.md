# Replacement ledger

Copy this file into the project (for example `docs/replacement-ledger.md`) and keep it current.

The example rows come from an imagined warehouse system. Replace them with your own.

## 1. Items from the old system

The **Kind** column takes one of four values:

- **purpose**: what users actually do with the old system
- **case**: a problem met and never solved
- **material**: data or a feed the new system can't regenerate
- **floor plan**: how the old system happens to be built

The **Verdict** column takes one of four values:

- **carry**: bring it into the new system
- **redo**: build it again, differently
- **transitional**: keep it only for the switchover, and give it a removal condition
- **unknown**: not decided yet; write down how and when it will be found out

A row whose Evidence cell is empty counts as **unknown**, whatever its Verdict says.

| ID | Item | Kind | Verdict | Evidence (what, how, when) | Premise | Falsifier | Depends on it |
|---|---|---|---|---|---|---|---|
| M1 | `stock_movements` table | material | carry, trusted from 2020-01-01 | `SELECT min(created_at), count(DISTINCT date(created_at))…` run 01-10: first row 2018-03-01, 412 days missing in 2018–19 | Movements after 2020-01 are complete | Any day after 2020-01 where opening count + movements ≠ closing count | S1, B1 |
| P2 | Finance month-end stock report | purpose | carry, output must match | Finance runs it monthly; 3 past outputs saved as golden files | Finance keeps the current valuation method | Finance changes the valuation method | B2 |
| C1 | Friday manual stock correction | case | redo: stock is derived from movements, never stored | 52 corrections last year, 70% in two SKU groups written by both the scanner app and the import job | Drift comes from two writers | Corrections continue after the one-writer rule | S1 |
| F1 | Nightly `recalc_counts` cron | floor plan | drop | Exists to dodge table locks in the old DB version (commit message, 2019) | The new store has no such locks | Lock waits show up in new-system metrics | — |
| T1 | Nightly CSV export to the reporting team | transitional | keep until reporting reads the new API; remove when its last reader moves | Two scripts on the reporting server read it | — | — | B2 |
| U1 | `supplier_contracts.legacy_flag` | unknown | Find out by 02-01: search for readers, ask purchasing | — | — | — | — |

## 2. Standard (checkable statements)

| ID | Statement | How it is checked | Serves |
|---|---|---|---|
| S1 | Stock on hand for any item, warehouse and day equals the sum of movements | Nightly invariant job; zero violations | C1, M1 |
| S2 | The month-end report matches the golden files for Oct–Dec | Golden-file test | P2 |

### Later (wishes with no purpose or case row yet)

| Wish | Why someone wants it | What would promote it |
|---|---|---|
| Plugin system | "For the future" | A named second integration |

## 3. Blocks

| Block | Mode | Oracle | Cutover gate | Switch-back |
|---|---|---|---|---|
| B1 Stock ledger | redesign | S1 invariant + hand-checked answer table for 20 SKUs | 10 clean days | Old app keeps writing; new one is shadow only |
| B2 Finance reports | mimic | Old report, diffed daily | 2 month-ends identical | Finance keeps the old report link |

## 4. Old-system watch

| Check | Schedule | Last run | New items found |
|---|---|---|---|
| Schema diff (tables, columns, triggers) | Weekly | | |
| New kinds of records or codes | Weekly | | |
| Cron and config diff | Weekly | | |
