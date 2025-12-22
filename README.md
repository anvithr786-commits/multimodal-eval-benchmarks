# multimodal-eval-benchmarks

Work-in-progress benchmark & evaluation suite for multimodal / generative media models — task sets, scoring metrics, and human preference rubrics.  
**Started:** Dec 18, 2025  
**Last updated:** Dec 22, 2025

## Progress log
- **Day 1:** defined benchmark scope + failure taxonomy; drafted v0.1 tasks; created human rubric, evaluation protocol, and metrics plan; added prompt set v0.1 (36 prompts) + reporting artifacts + quickstart.
- **Next 48 hours:** expand to v0.2 (harder prompts), add rater QA examples + calibration prompts, and attach real model outputs for a fully populated run report.

## Repo map (start here)
- **Tasks:** `tasks/task_catalog.md`
- **Prompts:** `tasks/prompts_v0.1.md`
- **Human rubric:** `rubric/human_rubric.md`
- **Evaluation protocol:** `eval/evaluation_protocol.md`
- **Quickstart (how to run):** `eval/quickstart.md`
- **Metrics plan:** `metrics/metrics_plan.md`
- **Reporting template:** `reporting/one_page_report_template.md`
- **Scoring sheet (blank):** `reporting/scoring_sheet.csv`
- **Dry run report:** `reporting/run_report_2025-12-18.md`
- **Dry run scoring sheet (example):** `reporting/scoring_sheet_example.csv`

## What a run produces
- Overall + per-task **win-rates** (pairwise preference)
- Rubric dimension scores: **adherence, temporal consistency, identity consistency, realism, edit precision**
- Failure-tag distribution + top examples (for fast iteration)
- **Artifacts per run:** a completed scoring sheet + a one-page report (stored under `reporting/`)

## Changelog
- **2025-12-18:** v0.1 initialized (tasks, rubric, protocol, metrics plan)
- **2025-12-18:** added prompt set v0.1 (36 prompts)
- **2025-12-18:** added reporting artifacts (one-page template, scoring sheets, dry-run report skeleton)
- **2025-12-18:** added quickstart guide for running an end-to-end eval
- **2025-12-22:** README polish + added dry-run artifacts and reporting/quickstart refinements
