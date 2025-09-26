# AI DEV RESEARCH 
**AIDev Dataset Challenge — Research Question 5 (a/b/c)**

This repository investigates **failure patterns**, **early textual signals**, and **security-related signals** in agentic pull requests (PRs) from the **AIDev** dataset, addressing **RQ 5(a), 5(b), and 5(c)** from the *AI Teammates in SE 3.0* preprint.

---

## Submission summary
- **Task:** AIDev Dataset Challenge — RQ 5(a), 5(b), 5(c)
- **Repository:** README, analysis notebook, requirements, and artifacts
- **Technical report:** `results/reports/AIDev_RQ5_Technical_Report.pdf`
- **Time to complete:** **13 working days** (**14 calendar days**), from **Sep 12 to Sep 25, 2025**

---

## Repository layout
```
AI DEV RESEARCH/
├─ AIDev_RQ5_Analysis.ipynb        # end-to-end analysis notebook
├─ results/
│  ├─ figures/
│  │  └─ assessment_gantt.png      # timeline figure
│  └─ reports/
│     └─ AIDev_RQ5_Technical_Report.pdf
├─ README.md
└─ requirements.txt
```

---

## Setup
Python **3.10+** recommended (tested on Colab 3.12).

```bash
python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

**Colab (install directly from GitHub):**
```python
!pip install -r https://raw.githubusercontent.com/<USER>/<REPO>/main/requirements.txt
```

---

## Data
- Hugging Face dataset: **https://huggingface.co/datasets/hao-li/AIDev**
- Primary split: `all_pull_request`
- Optional enrichment: `all_repository`, `all_user`

> If your environment requires auth for Hugging Face, run `huggingface-cli login` (or set `HF_TOKEN`).

---

## How to run
1. Open `AIDev_RQ5_Analysis.ipynb` in Jupyter/Colab.
2. Execute cells top→bottom:
   - Imports & configuration
   - Load AIDev PRs (+ optional repository/user tables)
   - **Feature engineering:** outcome `accepted`; early signal `clarify_strict`; 5(a) failure/path flags; 5(c) security flags; controls (`log_stars`, `pr_size_log1p`, language one‑hots)
   - **RQ 5(a):** prevalence, Δ-acceptance (pp), small controlled logits
   - **RQ 5(b):** descriptives, bootstrap CIs, regularized logit (Δ probability, AUC)
   - **RQ 5(c):** security prevalence/acceptance (+ optional time‑to‑merge) with controls
   - Writer cell: exports tables/figures and the PDF report into `results/`

---

## Reproducibility
- Seed: `RNG_SEED = 42`
- Python 3.10+
- Artifacts are written under `results/` (figures, tables, reports)

---

---

## Citation
Please cite both the dataset and the preprint if you use this work:
- **Dataset:** `hao-li/AIDev` — https://huggingface.co/datasets/hao-li/AIDev
- **Preprint:** SAIL Research, *AI Teammates in SE3.0: How Autonomous Coding Agents Are Reshaping Software Engineering*.

---

## License
MIT
