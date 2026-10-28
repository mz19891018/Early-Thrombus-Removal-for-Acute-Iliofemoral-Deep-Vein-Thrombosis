# Early-Thrombus-Removal-for-Acute-Iliofemoral-Deep-Vein-Thrombosis
Thrombus removal combined with anticoagulation within 7 days of diagnosis is associated with a lower risk of moderate-to-severe post-thrombotic syndrome (PTS) at 24 months, but carries a higher risk of major bleeding.
[README.md](https://github.com/user-attachments/files/34077035/README.md)
# Acute Iliofemoral DVT Target Trial Emulation — Analysis Code

## Study
This repository accompanies the four-center retrospective study of early thrombus removal (CDT and/or PMT, with or without iliac stenting) plus anticoagulation versus no thrombus removal within seven days of imaging-confirmed acute iliofemoral DVT. The available study cohort contains 740 individuals diagnosed between **2016-01-15 and 2024-10-19**.

The primary endpoint is a 24-month hierarchical composite prioritizing death, recurrent venous thromboembolism, ISTH major bleeding, moderate-to-severe post-thrombotic syndrome, and any post-thrombotic syndrome, summarized by a weighted win ratio. The analysis also reports weighted Aalen–Johansen cumulative incidences, risk differences, and sensitivity analyses.

## Contents

| File | Main role |
|---|---|
| `core.py` | Dataset loading, baseline covariates, cloning and censoring weights, weighted outcomes and pairwise win ratio |
| `analysis.py` | Main bootstrap analysis, sensitivity analyses, and landmark estimators |
| `descriptive.py` | Cohort characteristics, balance, and descriptive summaries |
| `hte.py` | Risk strata and exploratory treatment-effect heterogeneity |
| `sl_cf.py` | Cross-fitted SuperLearner AIPW and exploratory causal forest |
| `figs1.py` | Initial study figures and supplementary balance plots |
| `fig4.py` | Sensitivity/subgroup plots |
| `figcf.py` | Exploratory causal-forest figures |
| `dag.py` | Causal directed acyclic graph |
| `build.py` | Assembly of results from previously saved pickle objects |
| `render.js` | Optional rendering support; inspect before invoking |

## Data governance
Patient-level clinical data **are not included** in this public source archive. Use of institutional records must follow the applicable ethics approvals, patient confidentiality, and local data-transfer policies. Do not commit unredacted hospital databases or linkage keys. The source data are expected to be supplied by the authorized research team.

## Software
The manuscript reports Python 3.13 and NumPy, pandas, SciPy, scikit-learn, and EconML. Additional modules may be needed for plotting, model fitting, and document rendering. Exact pinned package versions are not present in this code-only archive; create a tested `requirements.txt`/lock file after validation in the original environment. A typical isolated environment can be initialized as follows:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
python -m pip install numpy pandas scipy scikit-learn matplotlib openpyxl econml
```

## Setup and reproducibility limitations
This archive contains **code files only**, without the required patient-level workbook and without previously generated `.pkl` result artifacts. In `core.py` the current constant `DATA = "/home/claude/work/data.xlsx"` is a machine-specific absolute path. Before use, change it to the permitted local data path (preferably using an environment variable); do not commit real data paths containing identifiers.

The scripts were originally written for a particular analysis environment and expect data columns matching `core.py` mappings and output pickle objects from earlier stages. As supplied, they do **not** constitute a self-contained, one-command reproduction package. Individual steps and configuration need to be confirmed against the original run logs before asserting fully reproducible results. `build.py` expects `res_main.pkl`, `res_sens.pkl`, `res_sub.pkl`, `res_desc.pkl`, `res_hte.pkl` and `res_slcf.pkl` to exist in the working directory.

### Suggested analysis sequence (after authorized data and dependencies are available)

1. Examine `core.py` (`load`, covariate definitions, variable mapping) and replace the absolute input path with an authorized local path.
2. Run a data dictionary and cohort/date audit and verify that all time zero and outcome dates precede the recorded end of observation.
3. Inspect `analysis.py` for the current CLI and execution guard; only run the main analysis after confirming resource requirements and bootstrap settings.
4. Run `descriptive.py` and the optional exploratory modules if their prerequisites are available.
5. Use the saved pickle objects as inputs to `build.py`, and render figures with `figs1.py`, `fig4.py`, `figcf.py`, and `dag.py` as appropriate.
6. Compare regenerated output with the final paper, particularly the 24-month win ratio, PTS risk differences, major bleeding counts and time-to-event definitions.

## Scientific interpretation
Cloning, artificial censoring, and inverse-probability weights can reduce treatment-timing bias under consistency, positivity, and no-unmeasured-confounding assumptions. The no-early-intervention strategy permits later clinically indicated rescue intervention. PTS onset is interval observed, and recorded event times for PTS reflect first positive assessment. The main hierarchy is sensitive to treatment-window and outcome-tier choices. Exploratory subgroup and causal-forest estimates should not be used as validated patient-level treatment recommendations.

## Publication / citation
Article: *Early thrombus removal in acute iliofemoral deep vein thrombosis: a target trial emulation* (International Angiology manuscript). Authors and final bibliographic details are supplied in the manuscript. Cite the final journal publication and the exact repository release or commit hash when available.

## Contact
Corresponding author: Tao Feng, Fourth Affiliated Hospital of Xinjiang Medical University. E-mail: hhlpwe@163.com.

## Licensing
No open-source license is asserted by this README. Include an explicit license only after all authorized code contributors agree to the reuse terms.
