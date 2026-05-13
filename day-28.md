# Task: Fix a Broken MLflow Project and Re-Run It

The xFusionCorp Industries ML platform team packages their training runs as MLflow Projects so any engineer can reproduce them with a single `mlflow run` invocation. A project has been pre-staged at `/root/code/trainer/`, but the first run from the lab startup already failed—the MLproject file carries a subtle command-line bug. Your task is to fix the bug and then run the project end to end twice so successful runs are recorded.


1. The MLflow tracking server is already running on port `5000`. The MLflow UI button at the top of the lab can be opened to view the dashboard; the `trainer` experiment already contains a *FAILED* run from the automated run that the lab startup triggered against the broken project.

2. `/root/code/trainer/` contains:

  - `MLproject` – The project descriptor (this file has the bug).
  - `train.py` – The trainer entry point. This file is correct and must not be modified.

3. The fix is confined to `MLproject`. Once repaired, two successful mlflow run invocations must be recorded in the trainer experiment:

  - One explicit call: `mlflow run . -e train -P n_estimators=200 -P max_depth=10 --env-manager=local` (run from `/root/code/trainer`).
  - One default call: `mlflow run . -e train --env-manager=local`.


4. The end state must include:

  - The `MLproject` command line invokes `train.py` with flag names that match `train.py`'s argparse declarations.
  - The `trainer` experiment contains at least two *FINISHED* runs whose `params.n_estimators` values differ (one at `200`, one at the default `100`).
  - The original *FAILED* run from the startup is still present – It must not be deleted, because the lab's diagnosis trail depends on it.


> The failed run's page in the MLflow UI and /tmp/mlflow-run-initial.log both carry the underlying error. Running mlflow run . manually from /root/code/trainer reproduces the same error directly in the terminal.

---

# Solution:

- The ask is to correct the MLflow configuration. It states *The trainer entry point. This file is
correct and must not be modified.*, and the issue is in the `./trainer/MLProject` file.

Refer to detailed info on configuring [MLProject file](https://www.mlflow.org/docs/latest/ml/projects/#mlproject-file-configuration).

- Open the MLFlow UI, and we can see one failed experiment run.

![failed-run](./assets/mlops-day28.png)

- Open the both the files (`./trainer/MLProject` and `./trainer/train.py`) in VSCode and explore them. We can see that the parameter for
`n_estimators` is misconfigured in `MLProject`. It has `n_est` declared in `command` block instead of `n_estimators` defined as
argument in `train.py`.

![train.py](./assets/mlops-day28-1.png)


![MLProject](./assets/mlops/day-28-1a.png)


- Update the argument from `n_est` to`n_estimators` in `MLProject`s `command` block.


- The next task is to run two sets of this experiments, one with default `n_estimators` value of
100, and another with `200`. `cd` into the `./trainer` and run the `train.py` with default values:

```
python3 ./train.py
```

![run-default](./assets/mlops/day28-2.png)

Then next run with `n_estimators` value set to `200`.

```
python3 ./train.py --n_estimators=200
```


![run-200-estimatiors](./assets/mlops-day28-2a.png)

- Now, on the UI we can see two succesful runs of these experiments with different `n_estimators`
value.

![verify-runs](./assets/mlops-day28-3.png)


That's it. Hit **Check**

