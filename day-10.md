# Task:

The xFusionCorp Industries ML team is adopting DVC so that datasets and model files are versioned separately from code. Initialise DVC inside the existing Git repository at `/root/code/fraud-detection/` and record the initialisation in Git.


1. A Git repository already exists at `/root/code/fraud-detection/` with an initial commit.

2. Initialise DVC inside that repository so that the standard `.dvc/` control directory and `.dvcignore` file are created alongside the existing Git working tree.

3. Stage every file that DVC produces during initialisation, and record them in a new Git commit with the message `Initialize DVC`.

Once initialisation is complete, the DVC extension will detect the new `.dvc/` directory and surface the **DVC TRACKED** section in the **EXPLORER** panel together with a `DVC` indicator in the bottom status bar.

---

# Solution:

**NOTE**: This task deals with [**Data Version Control (DVC)**](https://doc.dvc.org/). A Git like Open-source version control system for Data Science and Machine Learning projects. 


- First cd into the `fraud-detection` dir. The project is initialized with Git with one *Initial
commit*.

- Now initialize the project with DVC. Use `dvc` CLI by running `dvc init`.
See more commands with `dvc` or `dvc --help`

- Once the project is initialized with DVC, stage the `.dvc/` and `.dvcignore` file generated after initializing the project using Git CLI.

```
git add .dvc/ .dvcignore
```

- And, commit it using Git

```
git commit -m "Initialize DVC"


