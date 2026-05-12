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

This deals with version control of Dataset, Models that are dealt by DVC. refer to the [official
docs](https://doc.dvc.org/example-scenarios/versioning-data-and-models).

- cd into the `fraud-detection` directiry and explore all the files as defined in point-1, and 2. We
  should see the dataset in the defined directories.

- From the main branch (Current) tag it to `v1.0`

`git tag v1.0`

- Now, create a new branch called `v2-improved`. As the task requirement is *Do NOT delete
transactions_v2.csv*. we copy the v2 dataset to the tracked data in this branch:

```
cp data/raw/transactions_v2.csv data/raw/transactions.csv
```

- Now as we've added a new version of dataset, we need to inform DVC to track it (DVC tracks
datasets).

```
root@controlplane fraud-detection on  v2-improved ➜  dvc add data/raw/transactions.csv
100% Adding...|████████████████████████████████████████████████████|1/1 [00:00, 88.91file/s]
                                                                                            
To track the changes with git, run:

        git add data/raw/transactions.csv.dvc

To enable auto staging, run:

        dvc config core.autostage true
```


- Now, we need to re-run the pipeline and commit the changes:

```
root@controlplane fraud-detection on  v2-improved [!] ➜  dvc repro
'data/raw/transactions.csv.dvc' didn't change, skipping                                     
Stage 'process_data' didn't change, skipping                                                
Stage 'split_data' didn't change, skipping                                                  
Data and pipelines are up to date.``
```

- Add the new dataset meta file to Git history:

```
git add data/raw/transactions.csv.dvc

git commit -m "Add improved v2 version of dataset"
```



- Now Checkout to the main branch and use `dvc checkout` to restore the `v1` dataset on disk.

`git checkout main`

Then:

```
root@controlplane fraud-detection on  main [!] ➜  dvc checkout
Building workspace index                                         |7.00 [00:00, 1.22kentry/s]
Comparing indexes                                                |8.00 [00:00, 3.72kentry/s]
Applying changes                                                  |1.00 [00:00, 1.41kfile/s]
M       data/raw/transactions.csv
```


