# Task:

After training a model, the xFusionCorp Industries ML team wants DVC to surface metrics through `dvc metrics show` and the DVC extension's METRICS view. The fraud-detection pipeline already trains a model and writes a `metrics.json`, but DVC does not recognise the file as a metric. Wire it in correctly.


1. A project exists at `/root/code/fraud-detection/` with a three-stage DVC pipeline (`process_data`, `split_data`, `train`). The train stage runs `src/models/train.py`, which writes the model to `models/model.pkl` and metrics to `metrics.json`. Do not modify the Python files.

2. The `train` stage in `dvc.yaml` must declare `metrics.json` as a DVC metric output, not as a regular file output. The metric must be declared with cache: `false` so the JSON lives in Git for diff history rather than in the DVC cache.

3. Re-run the pipeline with `dvc repro` so the metric registration takes effect.

4. After your changes, `dvc metrics show` must report the accuracy and `f1_score` values from `metrics.json`.

> The DVC extension's METRICS section under the DVC view will surface the same values directly in the
editor once the metric is registered.``


--- 
# Solution:

This deals with [metrics in DVC](https://doc.dvc.org/command-reference/metrics).

- cd into `fraud-detection` directory, and explore `dvc.yaml`.
The tasks asks to *`dvc.yaml` must declare `metrics.json` as a DVC metric output*. Currently the
`metrics.json` is defined under `outs`. move it under new section called `metrics` as shown in DVC
officis docs link above.

```
stages:
  process_data:
    cmd: python src/data/process_data.py
    deps:
      - data/raw/transactions.csv
      - src/data/process_data.py
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - data/processed/clean_transactions.csv
      - src/data/split_data.py
    outs:
      - data/processed/train.csv
      - data/processed/test.csv

 train:
    cmd: python src/models/train.py
    deps:
      - data/processed/train.csv
      - src/models/train.py
    outs:
      - models/model.pkl
    metrics:    # Add new block
      - metrics.json:
          cache: false  # disable saving the metrics.json in DVC cache
```

**Note**: We also need to disable caching of metrics as we want the metrics.json to be in Gt
history, and not in DVC cache.

- After updating the `dvc.yaml` as above. Run the `dvc repro`, once the pipeline completes run,
check the metrics with `dvc metrics show` command.

You should see:

```
root@controlplane fraud-detection on  main [!?] ➜  dvc metrics show
Path          accuracy    f1_score
metrics.json  1.0         1.0
```

