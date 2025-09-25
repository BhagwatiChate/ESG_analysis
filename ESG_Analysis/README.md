# AI-Powered ESG Analytics Engine — Deliverables

This repository contains the finalized pipeline notebook and supporting docs to reproduce the ESG analytics workflow end-to-end.

## Repository Structure
```
ESG_Deliverables/
├─ notebooks/
│  └─ ESG_Pipeline.ipynb
├─ docs/
│  ├─ APPROACH.md
│  └─ LICENSE.md
├─ outputs/
│  └─ sample_summary_table.csv
├─ requirements.txt
└─ README.md
```

## Quickstart

1. **Create & activate a virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Launch Jupyter & run the pipeline**
   ```bash
   jupyter notebook notebooks/ESG_Pipeline.ipynb
   ```

4. **Data**
   - Place downloaded ESG/CSR PDF reports in a local folder of your choice.
   - The notebook lets you configure an input directory path and will parse + process all PDFs in that directory. You may need to change the paths in the notebook.
   - Outputs, including the cross-company summary table, will be written to `outputs/` by default.
   - The Output directory contains all the results generated.

## Notes
- Some optional packages (OCR) require system dependencies (e.g., Tesseract). If OCR is not required for your PDFs, you can skip installing them.
- GPU acceleration (PyTorch + CUDA) is optional but recommended for large models; CPU-only works for the POC.
