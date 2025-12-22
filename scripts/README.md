#!/usr/bin/env python3
"""
prepare_run.py

Creates a run folder with:
- a prompt manifest CSV (selected prompts from tasks/prompts_v0.1.md)
- a scoring sheet CSV pre-filled with sample_id/task_id/prompt_id

This is intentionally lightweight so it works for "proof-of-work" runs.
"""

from __future__ import annotations

import argparse
import csv
import re
from dataclasses import dataclass
from datetime import date
from pathlib import Path
from typing import List, Optional


PROMPT_SECTION_RE = re.compile(r"^##\s+(?P<section>.+)$")
PROMPT_LINE_RE = re.compile(r"^(?P<prompt_id>\d+)\)\s+(?P<text>.+)$")


@dataclass
class Prompt:
    task_id: str
    prompt_id: int
    text: str


def task_id_from_section(section: str) -> str:
    """
    Map section header text -> Task ID.
    Example section: "T1 — Multi-Constraint Prompt Adherence (Text-to-Video)"
    """
    m = re.match(r"^\s*(T\d+)\b", section.strip())
    return m.group(1) if m else "T?"


def parse_prompts(md_path: Path) -> List[Prompt]:
    if not md_path.exists():
        raise FileNotFoundError(f"Prompts file not found: {md_path}")

    prompts: List[Prompt] = []
    current_task = "T?"

    for raw in md_path.read_text(encoding="utf-8").splitlines():
        line = raw.strip()

        sec = PROMPT_SECTION_RE.match(line)
        if sec:
            current_task = task_id_from_section(sec.group("section"))
            continue

        m = PROMPT_LINE_RE.match(line)
        if m:
            prompts.append(
                Prompt(
                    task_id=current_task,
                    prompt_id=int(m.group("prompt_id")),
                    text=m.group("text").strip(),
                )
            )

    if not prompts:
        raise ValueError(
            f"No prompts parsed from {md_path}. Ensure prompts are formatted like '1) ...' under '## T1 ...' sections."
        )
    return prompts


def filter_prompts(
    prompts: List[Prompt],
    include_tasks: Optional[List[str]] = None,
    limit: Optional[int] = None,
) -> List[Prompt]:
    out = prompts
    if include_tasks:
        include_set = {t.strip() for t in include_tasks}
        out = [p for p in out if p.task_id in include_set]
    if limit is not None:
        out = out[: max(0, limit)]
    return out


def write_manifest_csv(prompts: List[Prompt], out_path: Path) -> None:
    out_path.parent.mkdir(parents=True, exist_ok=True)
    with out_path.open("w", newline="", encoding="utf-8") as f:
        w = csv.writer(f)
        w.writerow(["task_id", "prompt_id", "prompt_text"])
        for p in prompts:
            w.writerow([p.task_id, p.prompt_id, p.text])


def write_scoring_csv(prompts: List[Prompt], out_path: Path) -> None:
    """
    Matches reporting/scoring_sheet.csv header and pre-fills sample_id/task_id/prompt_id.
    """
    header = [
        "sample_id",
        "task_id",
        "prompt_id",
        "model_a",
        "model_b",
        "winner",
        "adherence_1to5",
        "temporal_1to5",
        "identity_1to5",
        "realism_1to5",
        "edit_precision_1to5",
        "primary_tag",
        "secondary_tags",
        "notes",
        "rater_id",
    ]

    out_path.parent.mkdir(parents=True, exist_ok=True)
    with out_path.open("w", newline="", encoding="utf-8") as f:
        w = csv.writer(f)
        w.writerow(header)
        for i, p in enumerate(prompts, start=1):
            sample_id = f"s{i:03d}"
            # Leave remaining fields blank for raters to fill.
            w.writerow([sample_id, p.task_id, p.prompt_id] + [""] * (len(header) - 3))


def main() -> None:
    ap = argparse.ArgumentParser()
    ap.add_argument(
        "--prompts",
        default="tasks/prompts_v0.1.md",
        help="Path to the prompts markdown file (default: tasks/prompts_v0.1.md)",
    )
    ap.add_argument(
        "--out_dir",
        default=None,
        help="Output directory (default: runs/YYYY-MM-DD)",
    )
    ap.add_argument(
        "--tasks",
        default=None,
        help="Comma-separated list of task IDs to include (e.g., T1,T2,T4). Default: include all.",
    )
    ap.add_argument(
        "--limit",
        type=int,
        default=None,
        help="Limit number of prompts selected (after filtering). Default: no limit.",
    )

    args = ap.parse_args()

    prompts_path = Path(args.prompts)
    all_prompts = parse_prompts(prompts_path)

    include_tasks = [t.strip() for t in args.tasks.split(",")] if args.tasks else None
    selected = filter_prompts(all_prompts, include_tasks=include_tasks, limit=args.limit)

    run_date = date.today().isoformat()
    out_dir = Path(args.out_dir) if args.out_dir else Path("runs") / run_date

    manifest_path = out_dir / "prompt_manifest.csv"
    scoring_path = out_dir / "scoring_sheet.csv"

    write_manifest_csv(selected, manifest_path)
    write_scoring_csv(selected, scoring_path)

    print(f"Created run folder: {out_dir}")
    print(f"- Prompt manifest: {manifest_path}")
    print(f"- Scoring sheet:   {scoring_path}")
    print(f"Selected prompts:  {len(selected)}")


if __name__ == "__main__":
    main()
