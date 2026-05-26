# Task: Version Datasets and Models Across Git Branches

The xFusionCorp Industries ML team keeps different dataset and model versions on different Git branches so that the team can roll between versions cleanly. Tag the current state as `v1.0`, produce a `v2-improved` branch based on a newer dataset, and confirm that switching back restores the original data.


1. A project exists at `/root/code/fraud-detection/` with a working DVC pipeline and the baseline `data/raw/transactions.csv` already tracked.

2. An improved dataset has been pre-staged at `/root/code/fraud-detection/data/raw/transactions_v2.csv` and is visible in the file explorer. Do not delete this file.

3. On the main branch, tag the current state as `v1.0`.

4. Create a new branch named `v2-improved`. Replace the tracked dataset with the contents of the `v2` file, re-track it with DVC, re-run the pipeline, and commit the changes.

5. Switch back to the `main` branch and use `dvc checkout` to restore the `v1` dataset on disk. The restored content must match the hash recorded by the `v1.0` tag.

> The DVC extension's DVC TRACKED section in the EXPLORER panel will reflect the current branch's tracked state—it should show different dataset hashes on main and v2-improved.


---
# Solution

This task deals with version-controlling datasets and models with DVC. Refer to the [official docs](https://doc.dvc.org/example-scenarios/versioning-data-and-models).

- Change into the `fraud-detection` directory and explore the files as described in points 1 and 2. You should see the datasets in the specified directories.

- From the current `main` branch, tag it as `v1.0`:

`git tag v1.0`

- Now create a new branch called `v2-improved`. Since the task requires *Do NOT delete `transactions_v2.csv`*, we copy the v2 dataset over the tracked data in this branch:

```
cp data/raw/transactions_v2.csv data/raw/transactions.csv
```

- Now that we have added a new version of the dataset, we need to inform DVC to track it:

```
root@controlplane fraud-detection on  v2-improved ➜  dvc add data/raw/transactions.csv
100% Adding...|████████████████████████████████████████████████████|1/1 [00:00, 88.91file/s]
                                                                                            
To track the changes with git, run:

        git add data/raw/transactions.csv.dvc

To enable auto staging, run:

        dvc config core.autostage true
```


- Now re-run the pipeline and commit the changes:

```
root@controlplane fraud-detection on  v2-improved [!] ➜  dvc repro
'data/raw/transactions.csv.dvc' didn't change, skipping                                     
Stage 'process_data' didn't change, skipping                                                
Stage 'split_data' didn't change, skipping                                                  
Data and pipelines are up to date.``
```

- Add the new dataset metadata file to Git history:

```
git add data/raw/transactions.csv.dvc

git commit -m "Add improved v2 version of dataset"
```



- Now check out the `main` branch and use `dvc checkout` to restore the `v1` dataset on disk:

`git checkout main`

Then:

```
root@controlplane fraud-detection on  main [!] ➜  dvc checkout
Building workspace index                                         |7.00 [00:00, 1.22kentry/s]
Comparing indexes                                                |8.00 [00:00, 3.72kentry/s]
Applying changes                                                  |1.00 [00:00, 1.41kfile/s]
M       data/raw/transactions.csv
```


