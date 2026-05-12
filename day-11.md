# Task: Track a Dataset with DVC

A teammate has added the transactions dataset to the xFusionCorp Industries fraud-detection repository, but it was committed directly to Git instead of being tracked with DVC. Bring the repository in line with the team standard—every dataset under data/ must be tracked by DVC, not by Git.


1. A project exists at `/root/code/fraud-detection/` with DVC already initialised. The dataset `data/raw/transactions.csv` is currently tracked by Git, and the team standard requires DVC to own it instead.

2. Stop Git from tracking the dataset without deleting it from disk.

3. Track the same dataset with DVC so a `.dvc` pointer file is produced and `data/raw/.gitignore` excludes the dataset itself.

4. Stage the new `.dvc` pointer and the new `.gitignore`, then record a Git commit with the message Track transactions dataset with DVC.

Once tracking is moved to DVC, the DVC TRACKED section in the EXPLORER panel will list the dataset, confirming the extension recognises it as a DVC-managed file.


---

# Solution:

This builds on the last task working with DVC. 

- cd into `fraud-detection` dir

- Explore the git logs and you would see that the transaction dataset is tracked in Git. The tasks
is to exclude it from git 

```
git rm -r --cached data/raw/transactions.csv
```

Without removing from Git tracking if you try to add it to DVC, the `dvc add` will error out
complaining the file is tracked by git.

- Once it's removed from git, add it to dvc:

```
dvc add data/raw/transaction.csv
```


- Once the transaction.csv is added to the dvc, stage the `.dvc` pointer and `.gitignore` using git 

```
git add data/raw/.gitignore data/raw/transactions.csv.dvc
```

This command would be presented when you add the transaction dataset with `dvc add` command.

- Commit with message *Track transactions dataset with DVC*



