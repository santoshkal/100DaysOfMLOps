# Task: Create a DVC Pipeline for Data Processing

The xFusionCorp Industries ML team uses DVC pipelines to keep data processing reproducible. A draft `dvc.yaml` exists in the fraud-detection project, but `dvc repro` does not complete the full pipeline. Correct the pipeline definition so it runs cleanly end to end.


1. A project exists at `/root/code/fraud-detection/` with DVC initialised. Python scripts are at `src/data/process_data.py` and `src/data/split_data.py`; raw input is at `data/raw/transactions.csv`. Do not modify the Python files or the input data.

2. The corrected pipeline must declare two stages with the following behaviour:

  - `process_data` – Depends on `data/raw/transactions.csv` and `src/data/process_data.py`; produces `data/processed/clean_transactions.csv`.
  - `split_data` – Depends on `data/processed/clean_transactions.csv` and `src/data/split_data.py`; produces `data/processed/train.csv` and `data/processed/test.csv`.

3. Review the existing `dvc.yaml` and correct everything that prevents dvc repro from completing.

4. After your changes, dvc repro must run end to end and dvc status must report no stale stages.

> Once the pipeline is valid, the DVC extension's PIPELINES section under the DVC view will list both stages and visualise the dependency graph between them.

---
# Solution:

This deals with [DVC Pipelines](https://doc.dvc.org/user-guide/pipelines).

The tasks says `dvc repro` fails. `dvc repo` command reproduces complete or partial pipelines by executing their stages.


- cd into the `fraud-etection` directory and explore the `dvc.yaml` pipeline config file. You can notice that the command `cmd` inone of the stage, and dependencies `deps` and outputs `outs` are misconfigured. 
Correct the configuration aligning with the requirements provided in point-2.

```
stages:
  process_data:
    cmd: python src/data/process_data.py # Udpate this
    deps:
      - data/raw/transactions.csv # Update this
      - src/data/process_data.py # Update this
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - data/processed/clean_transactions.csv  
      - src/data/split_data.py # Update this
    outs:
      - data/processed/train.csv
      - data/processed/test.csv
```

> refer to point-2 of the requirements for depndencies and outputs.

- Once updated, run the `dvc repro` command:

```
root@controlplane fraud-detection on  main [!] ✖ dvc repro 
Running stage 'process_data':                                                               
> python src/data/process_data.py
Processed 15 rows
Generating lock file 'dvc.lock'                                                             
Updating lock file 'dvc.lock'

Running stage 'split_data':                                                                 
> python src/data/split_data.py
Train: 12 rows, Test: 3 rows
Updating lock file 'dvc.lock'                                                               

To track the changes with git, run:

        git add data/processed/.gitignore dvc.lock

To enable auto staging, run:

        dvc config core.autostage true
Use `dvc push` to send your updates to remote storage.
```


Checking the status with `dvc status` should give you:

```
root@controlplane fraud-detection on  main [!?] ➜  dvc status
Data and pipelines are up to date.
```


