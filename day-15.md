# Task: Parameterize a DVC Pipeline

The xFusionCorp Industries ML team manages model hyperparameters through params.yaml so experiments can vary without code changes. The fraud-detection project's train stage already wires `params.yaml` for `n_estimators`, but `dvc repro` currently fails. Correct the parameter wiring and demonstrate that DVC re-runs the train stage when the parameter changes.


1. A project exists at `/root/code/fraud-detection/` with a three-stage DVC pipeline (`process_data`, `split_data`, `train`) and a `params.yaml` already in place. Do not modify the Python files.

2. The `train` stage in `dvc.yaml` references the `n_estimators` parameter. Every name listed under params: must resolve to a key in `params.yaml`.

3. Review `params.yaml`, correct whatever prevents `dvc repro` from completing, and run the full pipeline.

4. Demonstrate that DVC tracks parameter changes by updating `n_estimators` to a different value (for example `200`). Run `dvc repro` again—only the train stage should re-execute, the new value must be recorded in `dvc.lock`, and `models/model.pkl` must be regenerated.

> The DVC extension's PARAMS section under the DVC view will surface the values from params.yaml directly in the editor.

---

# Solution:

This task deals with DVC pipelines. Pipelines can also have parameters in stages. Refer to the [official docs](https://doc.dvc.org/user-guide/pipelines/defining-pipelines#parameter-dependencies).

The task says `dvc repro` fails. Let us investigate.

- Change into the `fraud-detection` directory and run `dvc repro`. The error is:

```
ERROR: failed to reproduce 'train': Parameters 'n_estimators' are missing from 'params.yaml'
```

Let us examine `params.yaml` and `dvc.yaml`.

- There is a parameter name mismatch. `dvc.yaml` references `n_estimators`, whereas `params.yaml` defines `n_estimator` (singular). Correct the `params.yaml` value to align with `dvc.yaml`.

- Now `dvc repro` succeeds. To demonstrate that DVC only re-runs the `train` stage when parameters change, update `params.yaml` with `n_estimators: 200` and run `dvc repro` again. You should see:

```
root@controlplane fraud-detection on  main [!?] ➜  dvc repro
Stage 'process_data' didn't change, skipping                                                                                 
Stage 'split_data' didn't change, skipping                                                                                   
Running stage 'train':                                                                                                       
> python src/models/train.py
Trained RandomForestClassifier with n_estimators=200
Updating lock file 'dvc.lock'
...
```

As shown above, only the `train` stage was re-executed this time.



