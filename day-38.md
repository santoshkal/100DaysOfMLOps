# Task: Build Modular Training Pipeline with Config-Driven Stages


The xFusionCorp Industries ML platform team runs a parallel-training bake-off on the fraud-detection model—the same estimators is trained twice, once on a single worker and once across every available CPU, and the MLflow Compare view surfaces the wall-time gap. A draft script exists at `/root/code/fraud-detection/src/models/train_parallel.py` but running it currently produces near-identical wall times for the 'serial' and 'parallel' runs, and the Compare view cannot distinguish the two configurations. Your task is to correct the script so the second run genuinely runs in parallel and every MLflow run records the number of workers it actually used.


2. The MLflow tracking server is already running on port `5000` . The MLflow UI button at the top of the lab can be opened to confirm—the dashboard loads with an empty parallel-training experiment.

3. The project layout under `/root/code/fraud-detection/`:

  - `data/train.csv` – A 5000-row synthetic binary-classification dataset (imbalanced roughly 70 / 30). Larger than the 200-row dataset used earlier in the section because the `n_jobs` speedup is only visible once there is enough work per tree.
  - `src/models/train_parallel.py` – The bake-off script. Data loading, MLflow experiment setup, wall-time measurement, `metrics.training_time_seconds` `logging`, and model persistence to `models/model.pkl` are already wired; corrections are required.
  - Open `src/models/train_parallel.py` in the VS Code editor, correct every issue that keeps the bake-off from meeting the release checklist, save, and run the script once.

4. The end state must include:

  - At least two runs exist in the `parallel-training` experiment on MLflow. Across the two runs, `params.n_jobs` takes the values `1` and -`1` (no run still carries the hardcoded "all").
  - Every run carries `metrics.training_time_seconds,` and the `n_jobs = -1` run is measurably faster than the `n_jobs = 1` run (at least 10 %).
A pickled model at `/root/code/fraud-detection/models/model.pkl.`

---

# Solution:

- The end state requires at least two runs in the `parallel-training` experiment on MLflow, where `params.n_jobs` takes the values `1` and `-1` (no run carries the hardcoded `"all"`). Additionally, every run must carry `metrics.training_time_seconds`, and the `n_jobs=-1` run must be measurably faster than the `n_jobs=1` run (at least 10% faster).

- Change into the `fraud-detection` directory and open `src/models/train_parallel.py` in VSCode.

- The variable `N_JOBS_VALUES` is incorrectly defined. It should be `[1, -1]`. Fix that. The task also says no run should carry the hardcoded `"all"`. However, on line 46 the code uses `"all"` as the parameter value. Update it to use the variable from the loop.

![update-train-parallel](./assets/mlops-day38.png)

- Run the script once and observe the behavior. The run with `n_jobs=-1` should be measurably faster (by more than 10%) than the run with `n_jobs=1`. Both runs should log `metrics.training_time_seconds` and `params.n_jobs` as desired.

![verify-run](./assets/mlops-day38-1.png)

- Verify the same on the MLflow server UI, then hit **Check**.






