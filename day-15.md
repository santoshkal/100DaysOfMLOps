# Task: Parameterize a DVC Pipeline

The xFusionCorp Industries ML team manages model hyperparameters through params.yaml so experiments can vary without code changes. The fraud-detection project's train stage already wires `params.yaml` for `n_estimators`, but `dvc repro` currently fails. Correct the parameter wiring and demonstrate that DVC re-runs the train stage when the parameter changes.


1. A project exists at `/root/code/fraud-detection/` with a three-stage DVC pipeline (`process_data`, `split_data`, `train`) and a `params.yaml` already in place. Do not modify the Python files.

2. The `train` stage in `dvc.yaml` references the `n_estimators` parameter. Every name listed under params: must resolve to a key in `params.yaml`.

3. Review `params.yaml`, correct whatever prevents `dvc repro` from completing, and run the full pipeline.

4. Demonstrate that DVC tracks parameter changes by updating `n_estimators` to a different value (for example `200`). Run `dvc repro` again—only the train stage should re-execute, the new value must be recorded in `dvc.lock`, and `models/model.pkl` must be regenerated.

> The DVC extension's PARAMS section under the DVC view will surface the values from params.yaml directly in the editor.

---

# Solution:

This tasks deals with DVC piplines, Pipelines can also have parameters in stages. Refer to the [official docs](https://doc.dvc.org/user-guide/pipelines/defining-pipelines#parameter-dependencies).

The task says `dvc repro` fails. Let's check this out.

- cd into the `fraud-detection` and run `dvc repro`. We get an error:

```
ERROR: failed to reproduce 'train': Parameters 'n_estimators' are missing from 'params.yaml'
```

Let's look into `params.yaml` and `dvc.yaml`

- We can see that there is parameter name mismatch. `dvc.yaml` referes to `m_estimators`, where as, the `params.yaml` defines `n_estimator` (Singular). Correct the `params.yaml` value to align with `dvc.yaml`. 

- Now, when we run, `dvc repro` it succeeds. Now we need to demonstrate that with changed parameters (n_estimators set to 200), only the `train` stage should run. Update the `params.yaml` with new values and  run `dvc repro`, and you should see:

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

As you can see, this time only the `train` stage was run.



