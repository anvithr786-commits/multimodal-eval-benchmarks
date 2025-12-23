# Scripts (v0.1)

These scripts make the benchmark runnable end-to-end:
- generate a dated run folder with a prompt manifest + scoring sheet
- aggregate rater results into a quick summary (wins + tags + averages)

## prepare_run.py
Creates `runs/YYYY-MM-DD/` with:
- `prompt_manifest.csv`
- `scoring_sheet.csv`

Example:
```bash
python scripts/prepare_run.py --limit 12 --tasks T1,T2,T3,T4,T5,T6
Expected output (date will vary):

Created run folder: runs/2025-12-22
- Prompt manifest: runs/2025-12-22/prompt_manifest.csv
- Scoring sheet:   runs/2025-12-22/scoring_sheet.csv
Selected prompts:  12

aggregate_scores.py
Summarizes a scoring CSV:
python scripts/aggregate_scores.py --scores reporting/scoring_sheet_example.csv


Note: Dry-run artifacts may contain TBD values (expected until outputs are generated).

Then commit with:
- **Commit message:** `Fix scripts README (usage + expected output)`
- ✅ Commit directly to main
::contentReference[oaicite:0]{index=0}









