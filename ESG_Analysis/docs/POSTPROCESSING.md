# Post-Processing & Global Summarization

This step combines all parsed claims from every PDF and synthesizes a concise, data-backed summary.

## Inputs
A CSV exported from the notebook with one row per claim, with columns:
- `Company`, `Sector`, `ESG Topic`, `Extracted Claim`,
- `Sentiment Score`, `Specificity Score`,
- `Greenwashing Risk Score (Optional)` (if computed).

## What this produces
- `outputs/cross_company_summary.csv` — Aggregates by topic & sector (counts, average sentiment, specificity, and optional risk).
- `outputs/executive_summary.md` — A narrative across all companies: strongest areas, risk hotspots, common topics, representative claims.
- `outputs/topic_briefs.md` — One brief per ESG topic with directional metrics.

## How to run (from the notebook)
```python
import pandas as pd
from src.postprocess_summary import run_global_summary

# Replace with the actual path your pipeline writes
INPUT_CSV = "outputs/final_claims_table.csv"

paths = run_global_summary(INPUT_CSV, output_dir="outputs")
paths
```

## Notes
- This is **model-agnostic**: it works with any claim table following the schema.
- For defensibility, the executive summary includes **representative claims** rather than generative paraphrases.
- If you prefer a single-file output for sharing, print `executive_summary.md` and `topic_briefs.md` to PDF.
