# Approach & Rationale

## 1) Architectural Overview
The pipeline ingests PDF sustainability/ESG/CSR reports, converts them into clean text segments with provenance (company, document, page), classifies each segment into ESG topics, and evaluates **sentiment** and **specificity**. Optionally, it computes a **Greenwashing Risk Score**. The system is implemented end-to-end in a single Jupyter notebook for reproducibility.

High-level stages:
1. **Ingestion & Parsing** — PDF -> structured corpus (handles multi-column layouts, headers/footers, and chunking).
2. **Normalization & Chunking** — de-duplication, language filtering, section-aware splitting.
3. **ESG Topic Classification** — Assigns one/more topics from a hybrid SASB/GRI taxonomy.
4. **Claim Analysis** — Sentiment (polarity) + Specificity (heuristics & linguistic cues).
5. **(Optional) Greenwashing Risk** — Composite score from sentiment positivity, specificity, hedging language, and forward vs backward-looking bias.
6. **Synthesis** — Produces a cross-company summary table for analysts.

## 2) Rationale for Choices

### Companies & Sectors
The notebook is parameterized to work with **3–5 publicly traded companies** from at least **two sectors**. You can set the `COMPANY_CONFIG` in the notebook to map a company to its sector and its PDF file path(s).

### ESG Framework
We use a **hybrid** approach: **SASB** to anchor financial materiality (industry-specific) and **GRI** topic definitions to improve granularity and explainability. This aligns the output with investor needs while preserving coverage across Environmental, Social, and Governance themes.

### NLP Techniques
- **Topic Classification:** A lightweight, auditable baseline (keyword/regex + TF-IDF+linear model) backed by an optional transformer-based zero-shot classifier for robustness on out-of-vocabulary phrasing.
- **Sentiment:** Off-the-shelf sentiment model (transformers) with a fallback to rule-based polarity for noisy text.
- **Specificity:** Heuristic score combining evidence signals: presence of numbers/percentages/units, date-bound targets, baselines, KPIs, standard references (e.g., Scope 1/2/3), and passive voice penalty.
- **Greenwashing Risk (Optional):** Weighted blend of (i) high positive sentiment, (ii) low specificity, (iii) hedging/boilerplate density, and (iv) forward-looking bias.

### Considered Alternatives & Trade-offs
- Fine-tuning domain models (e.g., ClimateBERT) vs. zero-shot: **zero-shot** = low engineering overhead and no labeling, while **fine-tuning** can yield higher accuracy but requires curated labels and compute.
- End-to-end LLM extraction vs. hybrid: **hybrid** improves transparency and auditability and reduces hallucination risk, while still allowing LLM assistance on hard cases.
- Heavy OCR vs. text extraction first: Text extraction is faster and simpler for digital PDFs; OCR only toggled when needed.

## 3) Limitations
- PDF parsing remains imperfect for highly designed layouts; some manual curation may be needed.
- Topic mapping can be ambiguous; disclosing confidence scores and providing traceability to source text mitigates this.
- Specificity scoring is heuristic and may mis-rank borderline cases; future iterations can add supervised calibration or prompt-based adjudication.
- Zero-shot models can be slow on CPU; batching and caching are used to keep runtime reasonable.

## 4) Future Work
- Add weak supervision (Snorkel-style) to train a small supervised classifier for specificity.
- Expand taxonomy coverage with sector priors from SASB mappings.
- Integrate a retrieval layer to attach citations (page coords/snippets) to each claim in the summary table.
- Evaluate with a small, manually labeled set for precision/recall on topics and calibration plots for specificity.
- Package the pipeline as a CLI with unit tests and CI.

## 5) Reproducibility
- The notebook includes deterministic seeds where applicable.
- The environment is pinned in `requirements.txt`. For stricter reproducibility, export an `environment.yml` from the resolved environment.
