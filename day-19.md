# Task: Build Complete DVC ML Pipeline with Remote Storage and Experiments

Complete the xFusionCorp Industries fraud-detection production DVC pipeline. Three stages are already wired in `dvc.yaml`, two remain, and the pipeline must finish as a reproducible, SeaweedFS-backed, `v1.0-tagged` release.


1. A project exists at `/root/code/ml-pipeline/` with Git and DVC initialised. The `params.yaml` is in place and the `.dvc/config` is pre-configured to push to the SeaweedFS bucket dvc-storage at `http://localhost:8333`.

2. The `ingest`, `validate`, and `preprocess` stages are already declared in `dvc.yaml`, but one of them contains an incorrect output path that prevents `dvc repro` from completing. Find and fix it.

3. The remaining two stages need to be added:

  - `train` – Depends on the preprocessed dataset and `scripts/train.py`; reads `n_estimators`, `max_depth`, `test_size`, and `random_seed` from `params.yaml`; outputs `models/model.pkl` and `data/processed/test_split.csv`; declares `metrics.json` as a DVC metric with `cache: false`.
  `evaluate` – Depends on `models/model.pkl`, `data/processed/test_split.csv`, and `scripts/evaluate.py`; outputs `reports/evaluation.json` declared with `cache: false`.

4. The two scripts you need are pre-staged at `/root/code/ml-pipeline/scripts-staging/train.py` and `scripts-staging/evaluate.py`. Copy them into `scripts/` before adding the sages.

5. Run the full pipeline with `dvc repro`, push the cache to the SeaweedFS remote with `dvc push`, and tag the current state as `v1.0`.

6. Commit every change to Git so the release is fully captured.

> Open the SeaweedFS Filer button at the top of the lab and navigate to /buckets/dvc-storage/ to confirm that the bucket holds the pushed artefacts under the files/md5/... layout.

---

# Solution:

- First cd into the `ml-pipeline` dir and expore all the files available.

- The tasks says there is some issue in the `dvc.yaml` as `dvc repro` is failing. Run `dvc repro`
and we can see the firs error:

```
ERROR: failed to reproduce 'preprocess': output 'data/processed/cleaned.csv' does not exist
```

If we check the file, its `clean.csv` and not `cleaned.csv` as defined in the dvc.yaml. Update it to
`clean.yaml` andmove to next task.

- Now for point-3, we need to add two more stages to the `dvc.yaml`:

```
stages:
    ingest:
    ...
    validate:
    ...
    preprocess:
    ...

    train:
        cmd: python scripts/train.py
        deps:
          - data/processed/clean.csv
          - scripts/train.py
        params:
          - n_estimators
          - max_depth
          - test_size
          - random_seed
        outs:
          - models/model.pkl
          - data/processed/test_split.csv
        metrics:
          - metrics.json:
              cache: false

      evaluate:
        cmd: python scripts/evaluate.py
        deps:
          - models/model.pkl
          - data/processed/test_split.csv
          - scripts/evaluate.py
        outs:
          - reports/evaluation.json:
              cache: false
```


- Copy these files as asked in point-4. `/root/code/ml-pipeline/scripts-staging/train.py` and `scripts-staging/evaluate.py` into `scripts/`

Once Copied and `dvc.yaml` is updated, run `dvc repro`


```
root@controlplane ml-pipeline on  main [!?] ✖ dvc repro
Stage 'ingest' didn't change, skipping                                                      
Stage 'validate' didn't change, skipping                                                    
Stage 'preprocess' didn't change, skipping                                                  
Running stage 'train':                                                                      
> python scripts/train.py
Trained: {'accuracy': 1.0, 'f1_score': 1.0}
Updating lock file 'dvc.lock'                                                               

Running stage 'evaluate':                                                                   
> python scripts/evaluate.py
Evaluation: {'accuracy': 1.0, 'f1_score': 1.0, 'precision': 1.0, 'recall': 1.0, 'test_samples': 4}
Updating lock file 'dvc.lock'                                                               

To track the changes with git, run:

        git add dvc.lock

To enable auto staging, run:

        dvc config core.autostage true
Use `dvc push` to send your updates to remote storage.
```


- Now we need to stage the `dvc.lock` and other files and push the cache and tag it `v1.0`

`git add .`


```
root@controlplane ml-pipeline on  main [!?] ➜  dvc push
Collecting                                                       |3.00 [00:00, 1.13kentry/s]
Pushing
3 files pushed
```


`git tag v1.0`

