# Task:

The xFusionCorp Industries ML team uses SeaweedFS as the shared S3-compatible object store for DVC-tracked data. A `.dvc/config` already declares a remote called `s3` for the fraud-detection project, but dvc push currently fails. Correct the configuration and push the tracked data into the SeaweedFS bucket.


1. A project exists at `/root/code/fraud-detection/` with DVC initialised and `data/raw/transactions.csv` already tracked.

2. SeaweedFS is already running on the controlplane:

  - S3 endpoint: `http://localhost:8333`
  - Filer UI: open the SeaweedFS Filer button at the top of the lab (forwarded port 8888) – buckets are visible under `/buckets/`.
  - Credentials: `weedadmin` / `weedadmin123` (already set in .dvc/config)
- Bucket name: `dvc-storage` (already created and visible in the Filer UI under `/buckets/dvc-storage`)

3. Review the existing `.dvc/config` and correct everything that prevents `dvc push` from succeeding. The remote called `s3` must:

  - point at the `dvc-storage` bucket using `s3://`;
  - use the correct SeaweedFS S3 endpoint URL;
  - be marked as the default remote.

4. Push the tracked data. After the push, the `dvc-storage` bucket in the SeaweedFS Filer UI must contain at least one object under the `files/md5/...` prefix.

---
# Solution

- This task focuses on configuring and working with [DVC with remote data source](https://doc.dvc.org/start#configuring-a-remote) and [DVC configuration](https://doc.dvc.org/user-guide/project-structure/configuration#config-file-locations).

- cd into the `fraud-detection` directory.

- Update the `config` file within the `./fraud-detection/.dvc`

```
['core']
    remote = s3

['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
```

**Note**: For complete options in the configuration file refer to the [official docs](https://doc.dvc.org/user-guide/project-structure/configuration#config-file-locations).

The `['core']` section of the vdc config sets the repository’s default remote to s3

The `['remote, "s3"']` configures the parameters for the S3 bucket with endpoint, bucket name and
credentials.



- After updating the config, you can check if everything is fine by listing the remote with `dvc
remote list` and verofy if the config is proper with `dvc doctor` command. 

- Once veriofied you can push with `dvc push`. If the push succeeds, you would see a hidden file
under `./fraud-detection/.dvc/cache` with `files/md5/...` prefix.
