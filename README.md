<h1 align="center">🧵 Cotton → Yarn Quality Trace</h1>

<p align="center">
  <em>Does the quality of cotton going into the blend determine the quality of the yarn coming out?</em><br>
  An end-to-end data science investigation on one month of real spinning-mill data.
</p>

<p align="center">
  <a href="https://shobhitexp.github.io/Cotton_Yarn_Quality_Analysis_Shobhit-Raj/">
    <img src="https://img.shields.io/badge/🔴_Live_Dashboard-View_Now-2ea44f?style=for-the-badge" alt="Live Dashboard">
  </a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/pandas-EDA_%2B_ETL-150458?logo=pandas&logoColor=white">
  <img src="https://img.shields.io/badge/scikit--learn-ML_Models-F7931E?logo=scikitlearn&logoColor=white">
  <img src="https://img.shields.io/badge/Chart.js-Dashboard-FF6384?logo=chartdotjs&logoColor=white">
  <img src="https://img.shields.io/badge/status-exploratory-blue">
</p>

---

## 📌 The Question

Arvind Limited operates one of India's largest spinning mills. Cotton is procured from multiple sources, blended together in **mixing groups**, and spun into yarn on the **40s Compact NS** line (machine ACM1). The yarn is then tested for quality.

> **We want to understand one thing: does the quality of cotton going into the blend determine the quality of the yarn coming out?**

This project explores that question honestly — building a full analytical pipeline, and being clear about what the data *can* and *cannot* prove.

---

## 🎯 TL;DR — The Honest Answer

| | |
|---|---|
| **Physically?** | ✅ **Almost certainly yes** — settled textile science says fibre strength, micronaire, uniformity and trash all shape yarn quality. |
| **Statistically, on *this* data?** | ⚠️ **Cannot be demonstrated** — the dataset lacks the traceability, sample size, and input variation needed to see the effect. |

The value of this analysis isn't a model that predicts yarn quality — it's a rigorous, transparent demonstration of **why one month of single-machine data can't answer the question yet, and exactly what to collect next.**

---

## 🔍 What the Data Showed

<table>
<tr><td width="50%" valign="top">

**📊 Scale of the data**
- `53` mixing groups (blend batches)
- `915` individual cotton lots
- `13` usable yarn tests (after cleaning)
- Window: **25 Mar → 25 Apr 2026**

</td><td width="50%" valign="top">

**🧪 The core obstacle**
- **No shared ID** between the two files
- Link had to be inferred from **dates only**
- Process lag is **not recoverable** from the data (correlations flip sign as the assumed lag changes)

</td></tr>
</table>

**Statistical verdict:** Across `56` cotton-vs-yarn correlation tests, **zero** were statistically significant after correcting for multiple comparisons (Bonferroni threshold `p < 0.00089`).

**Machine learning verdict:** Eight model families (Linear, Ridge, Lasso, KNN, Random Forest, Gradient Boosting, SVR, Baseline) were compared with **Leave-One-Out Cross-Validation**. Every model overfit — good training fit, but cross-validated R² at or **below zero**:

| Model (predicting yarn strength, RKM) | Train R² | Cross-val R² | Verdict |
|---|:---:|:---:|:---:|
| Linear Regression | 0.80 | −2.29 | Overfit |
| Gradient Boosting | 0.79 | −0.37 | Overfit |
| Random Forest | 0.60 | −0.15 | Overfit |
| Ridge (regularised) | 0.39 | −0.32 | No signal |
| Baseline (predict mean) | 0.00 | −0.17 | — |

> 💡 The gap between training and cross-validated fit is the classic signature of **overfitting**: with 8 correlated inputs and only 13 rows, any model memorises noise. This is a **data-volume problem, not a model-choice problem.**

---

## 🗂️ Repository Contents

| File | What it is |
|---|---|
| **[`index.html`](index.html)** | 🔴 Interactive dashboard — self-explanatory charts, glossary tooltips on every term, sortable tables. **[View it live ↗](https://shobhitexp.github.io/Cotton_Yarn_Quality_Analysis_Shobhit-Raj/)** |
| **[`Cotton_Yarn_Quality_Analysis.ipynb`](Cotton_Yarn_Quality_Analysis.ipynb)** | 📓 Full notebook: architecture → ETL → EDA → statistics → ML, with overfitting/outlier diagnostics and caveats |
| **`Mixing_Laydown.xlsx`** | Raw cotton mixing records (groups, lots, weighted-average fibre properties) |
| **`Compact_Yarn.xlsx`** | Raw yarn quality test results |

---

## 🧭 How the Analysis Is Structured

```
01 · Data Map      →  Understand the two files, their grains, and the missing key
02 · Cotton Input  →  Variety mix, fibre-property distributions, outlier checks
03 · Yarn Output   →  Test results vs. the mill's own STD/Tolerance limits
04 · Linkage       →  Date-based join + lag-sensitivity scan (the hard part)
05 · Statistics    →  Correlations, p-values, multiple-comparison correction, VIF
06 · ML Models     →  8 models × 4 targets, LOOCV, overfitting diagnosis
07 · Findings      →  Honest answer + five caveats + what to do next
```

---

## 🧬 The Fibre Properties (Glossary)

<details>
<summary><strong>Click to expand — what each cotton measurement means</strong></summary>

| Code | Meaning |
|---|---|
| **LEN** | Fibre length (mm) — longer fibres generally spin stronger, more even yarn |
| **MIC** | Micronaire — fibre fineness/maturity (optimal ≈ 3.5–4.9) |
| **STR** | Fibre bundle strength (g/tex) |
| **RD** | Reflectance degree (%) — brightness/whiteness |
| **+B** | Yellowness — colour grade component |
| **UNF** | Uniformity index (%) — length consistency |
| **SFI** | Short fibre index (%) — share of very short fibres (more = more defects) |
| **TRASH** | Trash content (%) — non-fibre plant matter |

**Yarn side:** RKM (strength), C.S.P. (count-strength product), U.C.V% (evenness), Total Imp. (imperfections), Hairiness, Classimat faults.

</details>

---

## 🚀 What I'd Do Next

1. **Get real traceability** — log which mixing group physically became which yarn lot, with stage timestamps. This removes the entire date-guessing layer.
2. **Extend the window** — a full quarter or two gives enough rows to actually validate a model instead of overfitting.
3. **Widen the input range** — this is one blend recipe all month; data across different cotton grades would let a real effect reveal itself.
4. **Log the confounders** — humidity, machine settings, operator/shift — so they can be controlled for instead of left as noise.

---

## ⚙️ Tech Stack

`Python` · `pandas` · `NumPy` · `scikit-learn` · `SciPy` · `Matplotlib` · `Seaborn` · `Chart.js` · `HTML/CSS/JS`

---

<p align="center">
  <sub>Built by <strong>Shobhit Raj</strong> · Data as of April 2026 · Single machine, single blend, one month — an exploratory snapshot, not a production model.</sub>
</p>
