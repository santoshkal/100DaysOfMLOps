# Task: Set Up Code Quality Tools for ML Code

The xFusionCorp Industries ML team enforces code quality with ruff and black on every pull request. The project at /root/code/fraud-detection/ currently fails both tools. Make it pass them.


The project at `/root/code/fraud-detection/` contains a `pyproject.toml` and sample sources under `src/`.

The corrected project must meet the following requirements:

`ruff` and `black` are both configured with a line length of 120.
`ruff` lint rule selection includes `E`, `F`, `W`, and `I`, and is declared under [tool.ruff.lint] – The schema required by ruff 0.1 and later.
Running `ruff check src/` from the project directory exits with status 0.
Running `black --check src/` from the project directory exits with status 0.
Review the existing configuration and source files, and correct everything that prevents the two commands above from exiting cleanly.

`ruff`, `black`, and `mypy` are already installed.

---

# Solution:

- Change into the `fraud-detection` directory and examine the `pyproject.toml` file.

- Update `pyproject.toml` according to the requirements. The final file should look like this:

```
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 120 # Update line length

[tool.ruff.lint] # Update this
select = ["E", "F", "W", "I"]  # Update this to add rules

[tool.black]
line-length = 120 # Update line length
```

- Once `pyproject.toml` is updated, check it with `ruff check ./src`. You may find errors such as imported packages not being used.

- Fix the errors with `ruff check ./src --fix`. Now when you run `ruff check ./src`, you should see:
```
root@controlplane ~/code/fraud-detection via 🐍 v3.12.3 ➜  ruff check ./src
All checks passed!
```

- Now check formatting with `black`. Use `black --check src/` and if everything is fine, you should see:

```
root@controlplane ~/code/fraud-detection via 🐍 v3.12.3 ➜  black --check ./src
All done! ✨ 🍰 ✨
5 files would be left unchanged.
```
