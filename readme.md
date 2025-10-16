# APS Failure Detection — Scania Trucks (Challenge@Stellantis)

This repository hosts the **APS (Air Pressure System) Failure Detection** project based on Scania Trucks data.  
The goal is to build a **clean, modular, and reproducible ML codebase** for fault detection and explainability in APS systems.

---

## 🎯 Goal
Ingest → Preprocess → Feature Engineering → Train → Evaluate → Explain  
All within a minimal, Pythonic structure designed for collaborative development and CI/CD integration.

---

## 🚀 Quickstart

```bash
# Create a virtual environment
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run tests to confirm setup
pytest -q

# (Optional) Explore notebooks
jupyter notebook notebooks/

## 🚀 Data

```bash
Source: Scania CV AB (2016) APS dataset — operational data from heavy-truck air-pressure systems.
Summary:

~60,000 training samples (≈59k negative, ≈1k positive)

~16,000 test samples

171 anonymized numeric attributes (sensor counters & histograms)

Missing values marked as na

Target column: class (1 = positive, 0 = negative)

Official cost metric:

Total_cost
=
10
×
FP
+
500
×
FN
Total_cost=10×FP+500×FN

Repository layout for datasets:

data/
├─ raw/              # Original CSVs — LFS tracked
├─ processed/        # Cleaned/preprocessed datasets — LFS tracked
└─ house_processed/  # In-house artifacts (≤ 10 MB): features, models, reports


## 🚀 Notes

```bash

Large files (CSV, models >10MB) must be tracked with Git LFS.

Put any generated artifacts from scripts/notebooks under data/house_processed/.

Avoid committing heavy files directly to Git history.

## Modules

```bash

All code lives under src/challenge/ as an installable package.

Module	Purpose
ingest/	Load Scania APS CSVs (aps_failure_training_set.csv, aps_failure_test_set.csv) + missing-value handling.
preprocess/	Cleaning, imputation, scaling, train/test schema consistency.
features/	Feature engineering: meta-features, ratios, PCA transforms.
novelty/	Models & scoring (LogReg, RF, XGBoost, IsolationForest) + cost evaluation utility.
visualize/	EDA helpers: PCA plots, confusion matrices, feature importance.
utils/	Configs, constants, paths, small helpers.

Tests live in src/tests/ with slim placeholders to grow with the project.


## How To Initialize

```bash
# Clone repository
git clone <your_repo_url>
cd aps-failure-scania

# Initialize Git LFS (once per machine)
git lfs install

# (If needed) Pull large data files
git lfs pull

# Set up environment & dependencies
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Verify installation
pytest -q

## Workflow Overview

Ingest – Load & sanity-check CSVs (types, missingness, target balance).

Preprocess – Impute, scale, and align train/test schemas; manage class imbalance.

Feature Engineering – Derive statistical features & PCA components.

Train – Train baseline & advanced models (LogReg, RF, XGBoost, IsolationForest).

Evaluate – Report Scania cost metric, confusion matrix, precision/recall.

Explain – Use SHAP/feature importance to interpret model behavior.

Test – Grow src/tests/ to keep the pipeline robust and CI-friendly.

## Continuous Integration

GitHub Actions runs on each push/PR to:

Install dependencies

Run tests (pytest)

Validate imports and basic pipeline execution

Report build status (✅/❌) on PRs to keep main stable

## Conventions

Python 3.10+

Keep notebooks light; put core logic in src/

Small, focused commits with clear docstrings

Track large datasets/models via Git LFS

## Project Notes

Cost-sensitive evaluation is central (high FN penalty vs FP).

Class imbalance requires careful handling (resampling, class weights, threshold tuning).

Keep house_processed/ artifacts small for easy CI and reviews.
