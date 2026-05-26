# Task: Track a Dataset with DVC

A teammate has added the transactions dataset to the xFusionCorp Industries fraud-detection repository, but it was committed directly to Git instead of being tracked with DVC. Bring the repository in line with the team standard—every dataset under data/ must be tracked by DVC, not by Git.


1. A project exists at `/root/code/fraud-detection/` with DVC already initialised. The dataset `data/raw/transactions.csv` is currently tracked by Git, and the team standard requires DVC to own it instead.

2. Stop Git from tracking the dataset without deleting it from disk.

3. Track the same dataset with DVC so a `.dvc` pointer file is produced and `data/raw/.gitignore` excludes the dataset itself.

4. Stage the new `.dvc` pointer and the new `.gitignore`, then record a Git commit with the message Track transactions dataset with DVC.

Once tracking is moved to DVC, the DVC TRACKED section in the EXPLORER panel will list the dataset, confirming the extension recognises it as a DVC-managed file.


---

# Solution:

This builds on the previous task working with DVC. 

- Change into the `fraud-detection` directory.

- Examine the git logs; you will see that the transactions dataset is tracked in Git. The task is to remove it from Git tracking.

```
git rm -r --cached data/raw/transactions.csv
```

If you try to add it to DVC without removing it from Git tracking first, `dvc add` will error out, complaining that the file is tracked by Git.

- Once it is removed from Git, add it to DVC:

```
dvc add data/raw/transactions.csv
```


- Once `transactions.csv` is added to DVC, stage the `.dvc` pointer file and `.gitignore` using Git:

```
git add data/raw/.gitignore data/raw/transactions.csv.dvc
```

This command is displayed when you add the dataset with `dvc add`.

- Commit with message: *Track transactions dataset with DVC*.



