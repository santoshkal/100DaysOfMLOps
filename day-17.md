# Task: Run and Compare DVC Experiments

The xFusionCorp Industries data science team compares multiple training runs with different hyperparameters using DVC experiments. Run three experiments that vary the `n_estimators` hyperparameter, identify the best-performing one, and promote it to the tracked workspace.


1. A project exists at `/root/code/fraud-detection/` with a parameterised DVC pipeline already in place. `params.yaml` contains `n_estimators: 100` and the baseline pipeline has been run once.

2. Run three DVC experiments, each with a different value for `n_estimators` across a reasonable range (for example `50`, `200`, and `500`). Each experiment should produce a fresh `metrics.json`.

3. Compare the experiments and choose the one whose `f1_score` is the highest.

4. Apply the chosen experiment to the workspace so its `n_estimators`, `metrics.json`, and `models/model.pkl` become the tracked state.

> The DVC extension's EXPERIMENTS section under the DVC view lists every experiment alongside its parameters and metrics, supports running fresh experiments through the + action, and applies a selected experiment to the workspace from the right-click menu—every operation in this lab can be performed either through the extension UI or with the equivalent dvc exp commands.


---

# Solution:


This task deals with running experiments using DVC's `dvc exp` command. Refer to the [official docs](https://doc.dvc.org/start/experiments/experiment-pipelines) for more information.

The second requirement says to run *experiments* using the `dvc exp run` command. The `--set-param` flag allows us to set different `n_estimators` values for each experiment.

The `dvc.yaml` and `params.yaml` are correctly configured.

- Change into the `fraud-detection` directory and start the experiments:

- Run three experiments, each with a different `n_estimators` value:

```
dvc exp run --set-param n_estimators=50 # 1st experiment
dvc exp run --set-param n_estimators=50 # 2nd experiment
dvc exp run --set-param n_estimators=50 # 3rd experiment
```

- Now that three experiments are complete, compare them and choose the one with the highest `f1_score`. This can be done with the `dvc exp show` command.

```
root@controlplane fraud-detection on  main [!?] ➜  dvc exp show
WARNING: Unable to find `less` in the PATH. Check out <https://man.dvc.org/pipeline/show> for more info.
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── 
  Experiment                 Created    accuracy   f1_score   n_estimators   data/processed/clean_transactions.csv   data/processed/train.csv           data/raw/transactions.csv          src/data/process_data.py           src/data/split_data.py             src/models/train.py               
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── 
  workspace                  -              0.85       0.83   500            16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7  
  main                       09:33 PM          !          !   100            -                                       -                                  -                                  -                                  -                                  -                                 
  ├── 70945b1 [wrath-axon]   09:49 PM       0.85       0.83   500            16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7  
  ├── cb75dfd [genic-ices]   09:49 PM       0.94       0.92   200            16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7  
  └── a9f7e2b [vapid-ford]   09:48 PM     0.9175     0.8975   50             16ee9b988c5a51591382422b56e11960        142467e5074926d5eb5e7154aa456c25   262600809db02a8f3b97351c93c27784   20dd83528aa4f1c811acc1999f29b6e0   a8a5e02e0ea8627d58fa9454aa11e2e6   dbf36dea4d172da6c087a24fbadd5ba7  
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

The table is somewhat mangled, but we can see that the experiment with `n_estimators: 200` has the highest `f1_score` of `0.92`. We choose this experiment and apply it:

```
root@controlplane fraud-detection on  main [!?] ➜  dvc exp apply genic-ices
Building workspace index                                                                                                                        |3.00 [00:00,  827entry/s]
Comparing indexes                                                                                                                              |8.00 [00:00, 4.37kentry/s]
Applying changes                                                                                                                                |4.00 [00:00, 1.62kfile/s]
Changes for experiment 'genic-ices' have been applied to your current workspace.
```

The experiment named `genic-ices` is the one with the highest `f1_score` of `0.92`.

No need to stage or commit any files further.
