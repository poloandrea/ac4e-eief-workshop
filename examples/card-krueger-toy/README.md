# Card–Krueger toy example

The one example everyone uses in the workshop: fast-food employment in New Jersey
(NJ) and eastern Pennsylvania (PA), before and after New Jersey raised its minimum
wage in April 1992.

The data are **synthetic teaching data**, not Card and Krueger's survey. Do not draw
research conclusions from them or add significance stars to the tables. What each
file contains, and every rule that drops stores, is in [`data_notes.md`](data_notes.md).

## What is in this folder

| File | What it is |
| --- | --- |
| `data/toy_fast_food.csv` | Clean 24-row panel (12 stores, 2 waves). Never edit. |
| `data/messy_fast_food_extract.csv`, `data/chain_metadata.csv` | Messy 47-row survey extract and a chain lookup table. Never edit. |
| `src/did_analysis.py` | Checks the clean panel, prints group means and the 2x2 comparison, and writes a short note. |
| `src/panel_workflow.py` | Cleans the messy extract step by step, prints the store count after each step, runs four versions of the estimate, and writes a checked LaTeX table. |
| `tests/` | The checks behind the check command. Never edit them to make a check pass. |
| `faults/` | Planted errors. Use them only on a separate copy (branch). |

## Workshop path

One file per concept. The concepts follow the workshop order.

| Concept | Open this |
| --- | --- |
| Agent vs chatbot | `src/did_analysis.py`: let the agent run it and quote the note |
| Version control | This README: the agent's edit appears as a list of changes to accept or reject |
| `AGENTS.md` boundaries | [`../../AGENTS.md`](../../AGENTS.md); ask the agent to edit `data/toy_fast_food.csv` and watch it refuse |
| To-do lists | [`../card-krueger-goals/goals/ck-robustness-checks.md`](../card-krueger-goals/goals/ck-robustness-checks.md) (plan only) |
| Skills: why | [`../../.claude/skills/replication-checker/SKILL.md`](../../.claude/skills/replication-checker/SKILL.md) |
| Create a Skill | [`../../templates/skill_template.md`](../../templates/skill_template.md) and the worked example [`sample-restrictions-note`](../../.claude/skills/sample-restrictions-note/SKILL.md) |
| Sub-agent vs Skill | [`../../.claude/agents/data-reviewer.md`](../../.claude/agents/data-reviewer.md) |
| Skill or sub-agent? | Rule of thumb in [`../../agent-harness/README.md`](../../agent-harness/README.md) |
| Using sub-agents | `data-reviewer` and `literature-reviewer` on this folder; compare the two reports |
| Orchestrating | [`../card-krueger-swarm/README.md`](../card-krueger-swarm/README.md) (writer and checker) |
| Issue-board swarm | [`ck-data-cleaning`](../card-krueger-swarm/issues/ck-data-cleaning.md) and [`ck-parallel-trends-figure`](../card-krueger-swarm/issues/ck-parallel-trends-figure.md) |
| Goal | [`../card-krueger-goals/goals/ck-replication-audit.md`](../card-krueger-goals/goals/ck-replication-audit.md) |
| Acceptance criteria | "Done when" lists in the issue cards; [`../../templates/issue_card.md`](../../templates/issue_card.md) |
| Verification | `faults/drifted_table.tex` and the check command below |

## Run

From the project folder:

```bash
python3 -m venv .venv
. .venv/bin/activate
python3 -m pip install -r requirements-lock.txt
python3 examples/card-krueger-toy/src/did_analysis.py
python3 examples/card-krueger-toy/src/panel_workflow.py
python3 -m pytest examples/card-krueger-toy/tests examples/mini-economics/tests
```

`did_analysis.py` should print an NJ change of 2.0, a PA change of −1.0 and a toy
DiD of 3.0 FTE workers. `panel_workflow.py` should print the counts 47, 43, 42 and
36 rows (18 stores) and a baseline DiD of 2.875.

A passing check command shows only that the code reproduces these teaching numbers.
The research design, parallel trends and any causal reading stay with the researcher.

## Which code makes which output

| Output | Function |
| --- | --- |
| Group means (clean panel) | `did_analysis.make_group_means` |
| Toy 2x2 comparison | `did_analysis.estimate_difference_in_differences` |
| Short note | `did_analysis.render_replication_note` |
| Store count after each step (messy extract) | `panel_workflow.clean_messy_extract` |
| Four-version table and the two-state warning | `panel_workflow.run_robustness_suite`, `panel_workflow.estimate_panel_did` |
| Checked LaTeX table | `panel_workflow.render_latex_booktabs_table`, `panel_workflow.verify_latex_table_numbers` |

## First prompt (read-only)

```text
Read examples/card-krueger-toy/README.md and data_notes.md. Do not edit files.
Run python3 examples/card-krueger-toy/src/did_analysis.py and quote the printed
NJ change, PA change and toy DiD. Then name the sample restriction in one sentence.
```

ciao
