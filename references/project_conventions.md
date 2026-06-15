# Project Conventions

Shared conventions that keep the hotel cancellation project reproducible, reviewable, and consistent across notebooks without adding unnecessary process overhead.

## Reproducibility

- Use a fixed `random_state` for train/test splits, cross-validation shuffling, and stochastic models. Default: `random_state=42`.
- Record the actual random state used in both the notebook narrative and the experiment log.
- Trust [requirements.txt](../requirements.txt) for library versions; record deviations only when a teammate reports a reproducibility drift.

## Naming

- Python variables, helper functions, and saved file names: lowercase with underscores.
- Notebook outputs and table labels: short, descriptive, stage-specific.
- Version labels for evolving artifacts: `feature_set_v1`, `prep_v2`, `model_v3`, etc.
- Artifact file names must make the stage obvious, e.g., `hotel_features_v1.parquet`, `baseline_results_v1.csv`.

## No Leakage Rule

- Do not use variables that would only be known after the cancellation outcome or after check-in/check-out.
- Known leakage-review columns in this dataset: `reservation_status`, `reservation_status_date`, `assigned_room_type`, `booking_changes` (see [references/dataset_links.md](dataset_links.md)).
- Fit imputers, scalers, encoders, and target-based transforms on training data only.
- Keep the test split fully isolated from feature design and preprocessing decisions that depend on the target.
- If a column has ambiguous timing or business meaning, mark it for leakage review before modeling and document the decision.

## Data Flow Between Notebooks

- Raw files live in `data/raw/` and are never modified in place.
- Cleaned intermediate artifacts are written to `data/interim/`.
- Modeling-ready feature matrices and splits are written to `data/processed/`.
- Each notebook states its inputs and outputs in its header so hand-offs are explicit.

## Experiment Recording

- Log every meaningful baseline or tuned run in [reports/tables/experiment_log_template.csv](../reports/tables/experiment_log_template.csv) (or a working copy).
- Required fields per run: `run_id`, `run_date`, `notebook_stage`, `model_name`, `feature_set_version`, `preprocessing_version`, `validation_strategy`, `cv_folds`, `decision_threshold`, `random_state`, metric columns, `primary_metric`, `secondary_metrics`, `notes`.
- Update the log when assumptions, thresholds, feature sets, or CV folds change.
- Keep one row = one run; do not aggregate multiple configurations into a single row.

## Figures And Tables Storage

- Report-ready figures: `reports/figures/` (PNG or SVG, descriptive names).
- Report-ready tables and experiment logs: `reports/tables/`.
- Draft interpretation and report fragments: `reports/drafts/`.
- Do not store large derived datasets in `reports/`.

## Notebook Etiquette

- Keep one logical concern per cell. Prefer splitting dense markdown and long code blocks.
- Every notebook starts with a standardized header block: title, pipeline role, rubric coverage, inputs, outputs, local lab references, key questions.
- Every notebook ends with a review checklist and a handoff note to the next notebook.
- Use `# TODO:` comments for work that still needs to be done; avoid silent gaps.
- Never fabricate outputs, metrics, or tables — if a result is not produced by the code, it does not go in the notebook.
- Rerun notebooks top-to-bottom before committing to confirm ordering is clean.

## Evaluation Defaults

- Primary evaluation metrics for this binary classification task: ROC-AUC, F1, precision, recall, accuracy. Report all of them where practical.
- Default validation protocol for tuning: stratified k-fold cross-validation with `random_state=42`.
- Default held-out test split: stratified 20% test set, frozen after [03_data_cleaning_and_preprocessing](../notebooks/03_data_cleaning_and_preprocessing.ipynb), used only in [07_final_model_evaluation](../notebooks/07_final_model_evaluation.ipynb).
