# Task: Production Training System: Tracking, Tuning, and Model Selection

The xFusionCorp Industries ML platform team ships a full fraud-detection training pipeline—data validation, Optuna tuning across two model families, model selection against a release threshold, Model Registry registration with a release-lane alias, and a consolidated training report—all wired together behind a single `make train-pipeline` command. The pre-staged system does not currently run end-to-end: each `make train-pipeline` invocation surfaces a wiring issue, and the integration across the `Makefile`, `src/select_model.py`, and `src/register.py` needs attention before the release checklist passes. Your task is to correct the wiring so `make train-pipeline` runs cleanly end-to-end, the MLflow Model Registry holds a fraud-detector version under the staging alias, and `reports/training_report.json` aggregates every upstream artefact.


1. The MLflow tracking server is already running on port `5000`. The MLflow UI button at the top of the lab can be opened to confirm—the dashboard loads with an empty `fraud-detection-tuning` experiment.

2. The project layout under `/root/code/fraud-detection/`:

  - `data/train.csv` – The 200-row synthetic binary-classification dataset the rest of the Training section uses.
  - `src/validate_data.py` – Schema + null-check gate. Writes reports/validation_status.json. Correct.
  - `src/tune.py` – Runs 10 Optuna trials across RandomForest and GradientBoosting, each logged as an MLflow run tagged with `model_type` + `params.{n_estimators,max_depth}` + `metrics.f1_score` + the fitted model artefact. Correct.
  - `src/select_model.py` – Picks the winning run by the training metric and writes `reports/selection.json.` Needs attention.
  - `src/register.py` – Registers the selected run's model as `fraud-detector` and assigns the release-lane alias. Needs attention.
  - `src/report.py` – Aggregates every upstream artefact into `reports/training_report.json.` Correct.
  - `Makefile` – `train-pipeline` target runs the five stages in order. Needs attention.

3. Run `make train-pipeline` from `/root/code/fraud-detection/` to surface each issue in turn. Open the offending file in the VS Code editor, correct the wiring, and re-run until the pipeline completes without non-zero exit.

4. The end state must include:

  - `make train-pipeline` completes without non-zero exit.
  - The `fraud-detection-tuning` MLflow experiment carries at least five trial runs, each with `metrics.f1_score.`
  - `reports/selection.json,` `reports/validation_status.json,` and `reports/training_report.json` are all present. The training report carries `best_model,` `best_params,` `metrics,` `total_trials,` and `validation_status` keys; `validation_status` is "ok" and `total_trials` is an integer ≥ 5.
  - The MLflow Model Registry (MLflow UI → Models) shows a `fraud-detector` registered model with at least one version. That version carries the `staging` alias and no `production` alias.


>  Run `make train-pipeline` once against the scaffold as-is; the first wiring issue surfaces immediately. Each subsequent re-run reveals the next stage's problem. Every fix is a one-line edit in one of the three files listed above.

---

# Solution:

- The task is to fix the pipeline run via `Makefile`. It is given that `src/select_model.py` and `src/register.py` have issues. Run `make train-pipeline` and observe the output:

```
root@controlplane ~/code/fraud-detection ➜  make train-pipeline
python3 src/validate_data.py
[validate] {'status': 'ok', 'rows': 200, 'columns': ['amount', 'hour', 'num_tx_past_day', 'is_fraud']}
python3 src/select_model.py
[select] no runs in experiment 'fraud-detection-tuning' — the tune stage has not produced any candidates yet.
make: *** [Makefile:8: train-pipeline] Error 1
```

The error log says *the tune stage has not produced any candidates yet* and exits with a non-zero exit code.

- Check the `Makefile`. The pipeline stages defined in each Python script (refer to the docstrings) differ from the stages defined in the `train-pipeline` target in the `Makefile`.
Looking at each Python script in `src`, the correct order is:
  - Stage 1: Data validation with `validate_data.py`
  - Stage 2: Optuna tuning across two model families with `tune.py`
  - Stage 3: Model selection with `select_model.py`
  - Stage 4: Register the selected model with `register.py`
  - Stage 5: Training report with `report.py`

![check-Makefile](./assets/mlops-day40.png)


- After correcting the `train-pipeline` target in the `Makefile`, re-run `make train-pipeline`. This time it errors with `KeyError: 'metrics.accuracy'` in `select_model.py`. Looking at `select_model.py`, it incorrectly references `metrics.accuracy` instead of `metrics.f1_score`. Update lines 31 and 41 in `select_model.py`.

![update-select-model](./assets/mlops-day40-1.png)

- Next issue: The MLflow server should have a `fraud-detector` registered model with at least one version carrying the `staging` alias and no `production` alias. Update the alias in `register.py` and re-run the pipeline with `make train-pipeline`.

![update-alias](./assets/mlops-day40-2.png)

![rerun-pipeline](./assets/mlops-day40-3.png)

- Verify in the MLflow server UI that the correct model is registered with the required alias, has at least five runs, and logs `metrics.f1_score`. Then hit **Check**.

![verify-alias](./assets/mlops-day40-4a.png)

![verify-runs](./assets/mlops-day40-4.png)








