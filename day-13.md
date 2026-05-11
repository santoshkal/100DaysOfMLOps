# Task:

A new xFusionCorp Industries team member has cloned the `fraud-detection` repository onto a fresh machine. The DVC remote is already configured to point at the team's SeaweedFS bucket, but `dvc pull` is failing. Diagnose the cause, correct the configuration, and pull the dataset.


1. A cloned project exists at `/root/code/fraud-detection/` with DVC initialised, the `data/raw/transactions.csv.dvc` pointer file present, but the dataset itself missing from disk and from the local DVC cache.

2. SeaweedFS is already running on the controlplane and the dataset has already been pushed to the dvc-storage bucket—open the SeaweedFS Filer button at the top of the lab and navigate to `/buckets/dvc-storage/` to confirm that the object is there.

  - S3 endpoint: `http://localhost:8333`
  - Credentials: `weedadmin` / `weedadmin123`


3. Review `.dvc/config` and correct everything that prevents `dvc pull` from authenticating against SeaweedFS.

4. After the fix, the s3 remote must use:
  - The access key (access_key_id) `weedadmin`
  - The secret key (secret_access_key) `weedadmin123`.

5. Pull the dataset. After the pull, `data/raw/transactions.csv` must be present on disk and its content must match the hash recorded in the `.dvc` pointer.

---

# Solution:

This deals with `dvc pull` command, and configs

- cd into the `fraud-detection` directory, and explore it. You can notice that the DVC cache
directory in `./fraud-detection/.dvc/data/raw` and `./fraud-detection/.dvc/cache` directory is missing. This is because we have not pulled it from
the remote. And, due to some reason, the `dvc pull` is failing. 

If we try `dvc pull` we see error:

```
root@controlplane fraud-detection on  main ➜  dvc pull
Collecting                                                        |1.00 [00:00,  738entry/s]
ERROR: failed to connect to s3 (dvc-storage/files/md5) - Unable to locate credentials
Fetching
ERROR: failed to pull data from the cloud - 1 files failed to download
```

You can see, it says *Unable to locate credentials*

- Explore the config in `fraud-detection/.dvc/config`, and you will see that the credentials are
missing in the config. Update it oaccorsdingly as provided in Point-3 of the task:

```
[core]
    remote = s3

['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
```

- After updateing the config, try `dvc pull` and you should succeed:

```
root@controlplane fraud-detection on  main [!] ✖ dvc pull
Collecting                                                        |0.00 [00:00,    ?entry/s]
Fetching
Building workspace index                                          |2.00 [00:00,  622entry/s]
Comparing indexes                                                |4.00 [00:00, 2.79kentry/s]
Applying changes                                                  |1.00 [00:00, 1.01kfile/s]
A       data/raw/transactions.csv
1 file fetched and 1 file added
```

- Now the `transactions.csv` should be present in `./fraud-detection/data/raw` dir.
