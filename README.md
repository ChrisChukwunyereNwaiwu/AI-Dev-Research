# AIDev Dataset Challenge — RQ 5(a/b/c): Do Clarification‑Seeking AI Agents Ship Better Code?

This repo contains the code and assets for my analysis of the **AIDev** dataset addressing
**Research Question 5** from the paper *The Rise of AI Teammates in SE 3.0*:

> **5(a)** Failure patterns & touched paths in agentic PRs  
> **5(b)** Early textual signals predicting merge acceptance  
> **5(c)** Security‑related signals and outcomes

The work reproduces the results reported in the accompanying DOCX/PDF and exports
tables/figures for submission.

---

## 📦 Contents

- `notebooks/`  
  - `AIDev_RQ5_Analysis.ipynb` — end‑to‑end analysis notebook (the Colab you ran).

- `results/` *(created at runtime)*  
  - Key figures: `acceptance_rate_bar.png`, `acceptance_by_language.png`, `acceptance_by_quartile.png`,  
    `5a_prevalence_top10.png`, `5a_acc_delta_top10.png`, `5c_acceptance_bar.png`  
  - Tables: `acceptance_by_clarify.csv`, `acceptance_by_language.csv`, etc.  
  - One‑pager: `results_5abc.md`  
  - Reports: `AIDev_5abc_Summary.docx`, `AIDev_RQ5_Report_v2.docx`, `AIDev_RQ5_Report_v2.pdf` *(if generated)*

- `README.md` — this file  
- `requirements.txt` — Python dependencies

---

## 🧰 Environment

- Python **3.10+** (tested on Colab, Python 3.12)  
- CPU is sufficient; no GPU required.

Install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

> If you use Google Colab, most packages are preinstalled. The notebook installs any missing ones as needed.

---

## 📊 Data

- Dataset (Hugging Face): **https://huggingface.co/datasets/hao-li/AIDev**  
- We primarily use the **pull request** split: `load_dataset("hao-li/AIDev", "all_pull_request")`.  
  Repository/user tables are optional and used only when present.

> If your environment needs auth for HF, run `huggingface-cli login` before executing the notebook.

---

## 🚀 How to Run

1) Open the notebook at `notebooks/AIDev_RQ5_Analysis.ipynb` (or import it into Colab).  
2) Run cells **top‑to‑bottom** in this order:
   - **Imports & config**  
   - **Load AIDev PRs**  
   - **Feature engineering** (build `accepted`, `clarify_strict`, failure/path flags, security flags, controls)  
   - **RQ 5(a)** — prevalence + Δ acceptance + small controlled logits  
   - **RQ 5(b)** — descriptives + bootstrap + regularized logit (Δprob, AUC)  
   - **RQ 5(c)** — security prevalence/acceptance + optional time‑to‑merge + controlled logit  
   - **Writer cell** — exports `results_5abc.md` and optional DOCX/PDF
3) Artifacts will be saved under `results/`.

---

## 🔍 Operationalization (high level)

- **Outcome:** `accepted ∈ {0,1}` via robust merge detection using any of (`merged` booleans, `merged_at` timestamps, `state/status="merged"`).  
- **Early signal (5b):** `clarify_strict = 1` if PR title/body (after stripping code blocks/URLs/inline code) contains a **question mark** and a **question word** (what/when/why/how/…).
- **Failure & paths (5a):** text flags `pat_docs`, `pat_build_ci`, `pat_lint`, `pat_revert_hotfix`, `pat_test_fail`; touched path flags `touch_docs`, `touch_tests`, `touch_src`, `touch_deps`, and `docs_only`.
- **Security (5c):** `security_flag = sec_text OR sec_dep_bump OR sec_secret_terms` (conservative keywords + dependency‑bump + secret terms).
- **Controls:** `log_stars` (repo popularity), `pr_size_log1p` (additions+deletions), `language` (one‑hot).

---

## 🧪 Methods Summary

- Descriptives with **bootstrap** (1,000 resamples) for acceptance‑rate gaps.  
- **Regularized logistic regression** (L2) for acceptance with controls; report **marginal Δ probability** (toggle `clarify_strict`) and **AUC**.  
- Per‑signal logits for 5(a) and 5(c) (associational, not causal).  
- Robustness: top languages, popular repos (stars ≥ 100).

> Seed: `RNG_SEED = 42` for reproducibility where sampling occurs.

---

## 📈 Expected Key Outputs

- **5(b)**: Overall acceptance, group rates for `clarify_strict` (0/1), raw Δ (pp), bootstrap 95% CI,
  regularized logit Δprob and AUC.  
- **5(a)**: Top‑prevalence failure/path signals and their acceptance deltas; per‑signal logits (with controls).  
- **5(c)**: Security prevalence and acceptance; (optional) time‑to‑merge medians; controlled logit summary.

---

## ⚠️ Notes & Limitations

- Heuristics are **lexical proxies** and may misclassify rhetorical/indirect cues; touched paths are coarse.  
- Associations are **not causal**; unobserved confounders may remain.  
- Timestamps may be coarse; same‑day merges can hide hours‑level differences.

---

## 📚 Citation

If you use this code, please cite the AIDev dataset and paper.

- Dataset: `hao-li/AIDev` — https://huggingface.co/datasets/hao-li/AIDev  
- Preprint: SAIL Research, *AI Teammates in SE3.0: How Autonomous Coding Agents Are Reshaping Software Engineering* (AIDev preprint).

---

## 📝 License

MIT License (feel free to adapt; update if your project uses a different license).
# AI-Dev-Research
