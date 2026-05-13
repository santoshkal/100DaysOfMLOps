# Task: Train a Scikit-Learn Model with Reproducible Script

The xFusionCorp Industries ML platform team maintains a config-driven training pipeline so hyperparameters can be swapped without editing Python code. A training scaffold exists at `/root/code/fraud-detection/` with the `trainer` already in place, but the YAML config has been left in a broken state and the pipeline will not run cleanly. Your task is to correct the config so one successful training run lands on the MLflow tracking server and the trained model ends up inside the project tree.



1. The MLflow tracking server is already running on port `5000`. The MLflow UI button at the top of the lab can be opened to confirm—the dashboard loads with an empty fraud-detection experiment already in place.

2. The project layout under `/root/code/fraud-detection/`:

  - `data/train.csv` – A pre-generated 200-row synthetic binary classification dataset (columns: `amount`, `hour`, `num_tx_past_day`, `is_fraud`).
  - `src/models/train.py` – The config-driven trainer. This file is correct and must not be modified.
  - `configs/train_config.yaml` – The project's training configuration. This file has bugs.
  - `models/` – Where the serialised model must land.


3. Running `python3 /root/code/fraud-detection/src/models/train.py` currently either fails with an estimator or column error, or succeeds while dropping the model file outside the project tree. Open `configs/train_config.yaml` in the VS Code editor, identify every setting that prevents a clean run, and correct it.

4. The end state must include:

  - A successful training run printed to stdout.
  - Exactly one new MLflow run in the `fraud-detection` experiment, with the estimator's hyperparameters logged as run parameters.
  - A serialised model at `/root/code/fraud-detection/models/model.pkl` (absolute path, inside the project tree).

> The trainer uses a small registry of supported estimators—`RandomForestClassifier`, `GradientBoostingClassifier`, `LogisticRegression`. Only these exact class names resolve.


---

# Solution:

- The issue is already provided in the description:
  - `src/models/train.py` has no issues
  - `configs/train_config.yaml` nneds to be looked into for errors

- Navigate to `fraud-detection` directory and explore `config/train_config.yaml` and `src/models/train.py` in VSCode, and try to run the `./src/models/train.py`. We first see an erro saying wrong estimator used:

```
ERROR: unknown estimator type 'RandomForest'. Supported: ['GradientBoostingClassifier', LogisticRegression', 'RandomForestClassifier']
```

We fis it with the one defined in the task with `RandomForestClassifier`, and re-run the `train.py`,
now wee see:

```
ERROR: target column 'target' not found in /root/code/fraud-detection/data/train.csv. Available columns: ['amount', 'hour', 'num_tx_past_day', 'is_fraud']
```

Here, we set the target_column as `is_fraud` based on the fact that this is the only column in the
dataset with binary calssification.

Now we set the `output.mde_path` as defined in the task and re-run `src/models/train.py` and we
should see the `model.pkl` generated and located in `./models` directory.


![config](./assets/mlops-day31.png)

Hit **Check**





