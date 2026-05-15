# Task: Automated Model Selection with FLAML AutoML

The xFusionCorp Industries ML platform team is running a three-way bake-off between a RandomForest, a GradientBoosting, and a LogisticRegression candidate for fraud detection, with every candidate tracked as an MLflow run in the `bakeoff` experiment. Three correct trainer scripts are already in place, but the orchestrator at `/root/code/fraud-detection/src/models/bakeoff.py` picks the wrong winner and writes an incomplete report. Your task is to correct the orchestrator so the saved winner is the `highest-F1` candidate and the report identifies which model family won.


1. The MLflow tracking server is already running on port `5000`. The MLflow UI button at the top of the lab can be opened to confirm—the dashboard loads with an empty bakeoff experiment.

2. The project layout under `/root/code/fraud-detection/`:

  - `data/train.csv` – The same 200-row synthetic binary-classification dataset Day 34 uses (imbalanced roughly 70 / 30).
  - `src/models/train_rf.py`, `src/models/train_gb.py`, `src/models/train_lr.py` – Three independent trainer scripts. Each one fits its named estimator with 3-fold stratified CV and logs one MLflow run tagged `candidate=<model family>` with the mean `f1_score` metric and its hyperparameters. These three files are correct and need no edits.
  - `src/models/bakeoff.py` – The orchestrator. It queries the bakeoff experiment with `mlflow.search_runs(...)` and writes `/root/code/fraud-detection/reports/winner.json`. Two specific corrections are required.


3. Run each of the three trainer scripts once so every candidate is logged, open `src/models/bakeoff.py` in the VS Code editor, correct the two problems that keep the report from meeting the release checklist, save, and run the orchestrator.

4. The end state must include:

  - Three runs exist in the `bakeoff` MLflow experiment, one per candidate, each with `tags.candidate`, the candidate's hyperparameters, and `metrics.f1_score`.
  - A JSON file at `/root/code/fraud-detection/reports/winner.json` with exactly three keys: `model_type` (one of `random_forest, `gradient_boosting`, `logistic_regression`), `run_id`, and `f1_score`.
  - The `model_type`, `run_id`, and `f1_score` stored in `winner.json` correspond to the candidate with the highest `f1_score` in the `bakeoff` experiment.


> The MLflow Compare view—select all three runs in the experiment's run list and click Compare—is the fastest way to eyeball which candidate won and spot-check the report.

--- 
# Solution:

**FLAML: A  Fast Library for Automated Machine Learning & Tuning**

- The give task is the orchestrator `backoff.py` picking the wrong winner, as we need to ensure `winner.json` correspond to the candidate with the highest `f1_score` and the second task is to rectify the generation of incomplete report.

- cd into the `fraud-detection` directiory and navidate to the `./src/models/bacloff.py`
When we see the `backoff.py`,we observer that the runs are filtered by `ASC` which orderes the runs
with *oldest first*, in order to filter based on *highest first* we need to update the line *39* in
`src/models/backoff.py` with `DESC` in `mlflow.search_runs.ordered_by` to order based on `highest_f1` score:

![order-by](./assets/mlops-day36.png)


- Next, we run all the trainer scripts `src/models/train_gb.py`, `train_lr.py`, and `train_rf.py` first, and then run `src/models/backoff.py`, to verify if the `./reports/winner.json` is populated with required metrics.

```
python3 src/model/train_gb.py

python3 train_lr.py

python3 train_rf.py

# Finally run the backoff script

python3 src/models/backoff.py
```


After running the scripts, we notice that the `./reports/winner.json` only captures `run_id`, and
`f1_score`. Where it need to captoure `model_type` as well,which is missing.

- We need to update the `backoff.py` script to add `model_type` as metric. We can do this where all
other metrics are define, i.e. at line *49* `reports` block.

The task also refers saying all the trainer scripts tag the model with `candidate=<model family>`.
We look at the trainer script and could see that on line *39* they set the model tag as `mlflow.set_tag("candidate", CANDIDATE)`. So the tag is `candidate`.
We update this on line *49* in `backoff.py`

![set-tag](./assets/mlops-day36-2.png)

- After setting the tag, we re-run the `backoff.py` and verify that the `./reports/winner.json`
is properly written with all the keys.

![verify-winner](./assets/mlops-day-36-3.png)

- We also need to ensure, that the MLFlow server has three runs of `backoff` experiment exist in the
Server UI, one per candidate, each with `tags.candidate`, the candidate's hyperparameters, and `metrics.f1_score`.

![verify-ui](./assets/mlops-day36-4.png)


Confirm, and hit **Check**

