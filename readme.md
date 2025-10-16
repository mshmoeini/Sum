APS Failure Detection – Scania Trucks

This repository hosts the APS (Air Pressure System) Failure Detection project based on Scania Trucks data.
The goal is to build a clean, modular, and reproducible machine learning codebase for fault detection and explainability in APS systems.

Goal

Ingest → Preprocess → Feature Engineering → Train → Evaluate → Explain
All within a minimal, Pythonic structure designed for collaborative development and CI/CD integration.

Quickstart
# Create a virtual environment
python -m venv .venv
source .venv/bin/activate   # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run tests to confirm setup
pytest -q

# (Optional) Explore the notebooks
jupyter notebook notebooks/

Data

The dataset comes from Scania CV AB (2016) and represents operational data from the Air Pressure System (APS) of heavy trucks.

60,000 training samples (59,000 negative, 1,000 positive)

16,000 test samples

171 anonymized numerical attributes (sensor counters and histograms)

Missing values represented as "na"

Target column: class (pos = 1, neg = 0)

Official evaluation metric:

Total_cost = 10 × (False Positives) + 500 × (False Negatives)


Data folders are organized as follows:

data/
├─ raw/              # Original CSV files (LFS tracked)
├─ processed/        # Cleaned or preprocessed datasets (LFS tracked)
└─ house_processed/  # In-house generated results and model artifacts (≤10 MB)


Notes:

Large files are tracked via Git LFS.

Use data/house_processed/ for derived outputs created by scripts or notebooks.

Avoid committing large datasets directly.

Modules

All code lives under /src/challenge/, structured as an installable and importable Python package.

Module	Purpose
ingest/	Load Scania APS CSV data (aps_failure_training_set.csv, aps_failure_test_set.csv) and handle missing values.
preprocess/	Cleaning, imputation, feature scaling, and train/test consistency.
features/	Feature engineering (meta-features, ratios, PCA transformations).
novelty/	Machine learning models (LogReg, RF, XGBoost, IsolationForest) and cost evaluation.
visualize/	Utilities for EDA, PCA plots, confusion matrices, and feature importance.
utils/	Shared helpers for configs, constants, and paths.

Testing modules live under /src/tests/, with initial placeholders ready for extension.

How To Initialize
# Clone repository
git clone <your_repo_url>
cd aps-failure-scania

# Initialize Git LFS
git lfs install

# Pull large data files
git lfs pull

# Set up environment and dependencies
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Verify installation and run tests
pytest -q

Workflow Overview

Ingest: Load and clean APS data from Scania CSV files.

Preprocess: Handle missing values, scale features, manage class imbalance.

Feature Engineering: Build derived statistical and PCA-based features.

Train: Fit models such as Logistic Regression, Random Forest, XGBoost, or Isolation Forest.

Evaluate: Compute Scania cost metric, confusion matrix, and precision/recall.

Explain: Apply SHAP or feature importance to interpret model behavior.

Test: Add or expand tests in /src/tests/ for reliability and CI stability.

Continuous Integration

Each push or pull request triggers GitHub Actions (CI) which:

Installs dependencies

Runs all tests automatically (pytest)

Validates imports and pipeline execution

Reports build status with a success or failure badge under each PR

Conventions

Python ≥ 3.10

Keep notebooks minimal; main logic should reside under /src/

Write clear docstrings and small, modular commits

Large data files must be handled with Git LFS
