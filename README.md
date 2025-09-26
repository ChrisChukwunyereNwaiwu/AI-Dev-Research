# AI DEV RESEARCH 

**AIDev Dataset Challenge: Studying AI Coding Agents on GitHub?**

This repository contains code, notebooks, and report artifacts for analyzing the **AIDev** dataset to answer **Research Question 5** from *AI Teammates in SE 3.0*:

- **5(a)** Failure patterns & touched paths in agentic PRs  
- **5(b)** Early textual signals predicting merge acceptance  
- **5(c)** Security-related signals and outcomes


## 📦 Repository Layout

```
AI DEV RESEARCH/
├─ notebooks/
│  └─ AIDev_RQ5_Analysis.ipynb          # end-to-end analysis notebook
├─ results/                              # created at runtime (figures, tables, reports)
├─ README.md
└─ requirements.txt
```


## 🛠️ Setup (requirements.txt at the repo root)

Place **`requirements.txt`** at the **root** of this repository (same level as `README.md`).  
Install dependencies locally or in a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

**Colab (install directly from GitHub):**
```python
!pip install -r https://raw.githubusercontent.com/<USER>/<REPO>/<BRANCH>/requirements.txt
```

> Python **3.10+** recommended (tested on Colab 3.12).


## 📊 Data

- Hugging Face dataset: **https://huggingface.co/datasets/hao-li/AIDev**  
- Primary split used: `all_pull_request` via `load_dataset("hao-li/AIDev", "all_pull_request")`  
- Optional enrichment: `all_repository`, `all_user`

> If your environment requires auth for Hugging Face, run: `huggingface-cli login` (or set `HF_TOKEN`).


## 🚀 How to Run

1. Open `notebooks/AIDev_RQ5_Analysis.ipynb` in Jupyter/Colab.  
2. Execute cells **top → bottom**:
   - Imports & configuration  
   - Load AIDev PRs (and optional repo/user tables)  
   - **Feature engineering:**  
     - Outcome `accepted` via robust merge detection (booleans, timestamps, state)  
     - Early signal **`clarify_strict`** (question mark **+** question word; code/URLs stripped)  
     - 5(a) failure/path flags; 5(c) security flags  
     - Controls: `log_stars`, `pr_size_log1p`, one-hot `language`
   - **RQ 5(a):** prevalence, Δ-acceptance (pp), small controlled logits  
   - **RQ 5(b):** descriptives, bootstrap (1,000), regularized logit (Δ probability, AUC)  
   - **RQ 5(c):** security prevalence/acceptance (+ optional time-to-merge), controlled logit  
   - Writer cell: exports tables/figures and DOCX/PDF under `results/`
3. Generated artifacts appear in `results/`.


## 🔍 Operationalization (high level)

- **Outcome:** `accepted ∈ {0,1}` from merged booleans / `merged_at` / `state=status="merged"`  
- **Early signal (5b):** `clarify_strict = 1` if title/body contains **?** and a question word *(what/when/why/how/should/could/would/which/if)* after removing code blocks, URLs, and inline code  
- **5(a) patterns & paths:** `pat_docs`, `pat_build_ci`, `pat_lint`, `pat_revert_hotfix`, `pat_test_fail`; touched-path flags `touch_docs/tests/src/deps`, plus `docs_only`  
- **5(c) security:** `security_flag = sec_text OR sec_dep_bump OR sec_secret_terms`  
- **Controls:** `log_stars`, `pr_size_log1p`, and one-hot `language`


## 🧪 Methods Summary

- Descriptive statistics with **bootstrap** CIs (1,000 resamples) for acceptance deltas  
- **Regularized logistic regression (L2)** for acceptance with controls; report **Δ probability** by toggling `clarify_strict` and the **AUC**  
- Robustness checks: top languages; popular repos (e.g., stars ≥ 100)  
- Reproducibility: set `RNG_SEED = 42` where sampling occurs

> Results are **associational**, not causal; unobserved confounding may remain.


## 📚 Citation

Please cite the dataset and paper if you use this repository:

- **Dataset:** `hao-li/AIDev` — https://huggingface.co/datasets/hao-li/AIDev  
- **Preprint:** SAIL Research, *AI Teammates in SE3.0: How Autonomous Coding Agents Are Reshaping Software Engineering* (AIDev preprint).


## 📝 License

MIT — feel free to adapt for your own research. If you need a different license, update this section accordingly.
