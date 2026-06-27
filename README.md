# Kepler Exoplanet Classification

A machine learning project that uses a **Random Forest classifier** to predict
whether a Kepler Object of Interest (KOI) is a real exoplanet or a false positive,
using NASA's Cumulative Kepler KOI dataset.

Built as a final project for DS 100 at the University of San Francisco.

---

## Background

NASA's Kepler Space Observatory (2009–2018) monitored over 150,000 stars for small
dips in brightness caused by planets passing in front of them — a phenomenon called
a **transit**. Objects flagged this way are called Kepler Objects of Interest (KOIs)
and are categorized as:

- **Confirmed Planet** – scientifically verified
- **Candidate** – unconfirmed, needs further study
- **False Positive** – not actually a planet

With limited telescope time and funding, scientists need a way to prioritize which
candidates are worth investigating. This project builds a model to help with that.

---

## Research Question

> Given the measurable properties of a KOI, can we predict whether it is likely
> to be a real exoplanet or a false positive?

---

## Dataset

**Source:** [NASA Exoplanet Archive – Cumulative KOI Table](https://exoplanetarchive.ipac.caltech.edu/)

- **9,564 KOIs** with 50+ features each
- Features include transit depth/duration, orbital period, stellar radius,
  temperature, signal-to-noise ratio, and contamination flags
- Missing values handled via **median imputation**
- After cleaning: 9,564 rows × 47 columns, no missing values

The target variable was binarized:
- `1` = Real planet (Confirmed or Candidate)
- `0` = False positive

---

## Methodology

We trained a **Random Forest classifier** (100 trees, 80/20 train-test split).

Random Forest was chosen because it:
- Handles noisy astronomical data well
- Reduces overfitting through ensemble averaging
- Outputs probability scores, not just 0/1 labels
- Is widely used and trusted in scientific research

---

## Results

| Metric | Score |
|---|---|
| Training Accuracy | 1.0000 |
| Test Accuracy | 0.9793 |

- Applied the trained model to **2,248 unconfirmed candidate KOIs**
- **1,249 (55.6%)** predicted as real planets
- **999 (44.4%)** predicted as false positives
- Generated ranked confidence scores to help astronomers prioritize follow-up

### Prediction Stability
To measure variability, we trained **50 models with different random seeds** and
tracked `planet_frac` (fraction of models that predicted "planet") and `flip_count`
(how often predictions changed). KOIs near 0% or 100% were highly stable;
those near 50% are ambiguous and warrant closer study.

### Key Feature Correlations
- **Positive** (associated with real planets): measurement uncertainties in stellar
  temperature, surface gravity, and transit duration
- **Negative** (associated with false positives): contamination flags
  (`koi_fpflag_co`, `koi_fpflag_ec`, `koi_fpflag_ss`) indicating eclipsing
  binaries or nearby star interference

---

## Files

| File | Description |
|---|---|
| `kepler_exoplanet_classifier.ipynb` | Full analysis: data cleaning, modeling, visualizations |
| `DS100_FinalProject.pdf` | Written report with methodology and findings |

---

## Tech Stack

- Python
- pandas, NumPy
- scikit-learn (RandomForestClassifier)
- matplotlib

---

## Authors

- **Aman** – Background, data cleaning, methodology, analysis, notebook
- **Naiem** – Analysis, model interpretation, limitations, conclusion, notebook
- **Lucas** – Presentation slides