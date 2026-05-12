# Task: Configure Pre-Commit Hooks for ML Repository

The xFusionCorp Industries ML team enforces code quality on every commit via `pre-commit`. A draft `.pre-commit-config.yaml` exists in the git repository at `/root/code/fraud-detection/`, but it does not match the team's standard and `pre-commit run --all-files` fails against it. Correct the configuration.


1. A git repository already exists at `/root/code/fraud-detection/` with `.pre-commit-config.yaml` and `process.py` already tracked. `pre-commit` is installed system-wide.

2. The corrected configuration must declare the following five hooks so that `pre-commit run --all-files` executes every one of them:

- `trailing-whitespace`, `end-of-file-fixer`, and `check-yaml` – All three sourced from the `pre-commit/pre-commit-hooks` repository, pinned to a current release;
- `ruff` – Sourced from the `astral-sh/ruff-pre-commit` repository, pinned to a current release;
- `black` – Sourced from the `psf/black-pre-commit-mirror` repository, pinned to a current release.

3. Every repository entry in the configuration must include a rev: field.

4. Review the existing `.pre-commit-config.yaml` and correct everything that prevents the hooks above from running.

5. Once the configuration is correct, register the hooks with git and run them against the tracked files:

```
   pre-commit install
   pre-commit run --all-files
```


Tip: pre-commit autoupdate queries each referenced repository and rewrites the rev: pins to the latest released tag. This is the standard way to discover current versions without looking them up by hand.

---

# Solution:
**Note**: This task deals with configuring [pre-commit hooks on a python project](https://pre-commit.com/)

- First cd into the `fraud-detection` dir, and check the `.pre-commit-config.yaml`. This is the file
  which defines all the pre-commit hooks for the repo. If we run `pre commit run --all-files` we
might see something like this:

```
root@controlplane fraud-detection on  main via 🐍 v3.12.3 ➜  pre-commit run --all-files
An error has occurred: InvalidConfigError: 
==> File .pre-commit-config.yaml
==> At Config()
==> At key: repos
==> At Repository(repo='https://github.com/psf/black-pre-commit-mirror')
=====> Missing required key: rev
Check the log at /root/.cache/pre-commit/pre-commit.log
```

- Update the `pre-commit-config.yaml` as following:

```
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0 # Check the latest version from the above repo
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml # Update from check_yaml to check-yaml

  - repo: https://github.com/astral-sh/ruff-pre-commit # Udpate to the repo as required
    rev: v0.15.12 # Check the version from the above github repo
    hooks:
      - id: ruff

  - repo: https://github.com/psf/black-pre-commit-mirror # Udpate to the repo as required
    rev: 26.3.1  # Note there is no 'v'. Check the latest version from the above repo
    hooks:
      - id: black
```

- Run `pre commit run --all-files`:

```
root@controlplane fraud-detection on  main [!] via 🐍 v3.12.3 ✖ pre-commit run --all-files
[INFO] Initializing environment for https://github.com/psf/black-pre-commit-mirror.
[INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
[INFO] Installing environment for https://github.com/astral-sh/ruff-pre-commit.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
[INFO] Installing environment for https://github.com/psf/black-pre-commit-mirror.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
trim trailing whitespace.................................................Failed
- hook id: trailing-whitespace
- exit code: 1
- files were modified by this hook

Fixing process.py

fix end of files.........................................................Passed
check yaml...............................................................Passed
ruff (legacy alias)......................................................Passed
black....................................................................Passed
```


