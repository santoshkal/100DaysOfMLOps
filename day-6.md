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

- cd into the `fraud-detection` dir and check the `pyproject.toml` file.

- Update the `pyproject.toml` according to the requirements, and the final file should look as
following:

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

- Once the `pyproject.toml` is updated. check this with ` ruff check ./src`. You could find some
error, like th epackage defined but not used. 

- Fix the errors with ` ruff check ./src --fix`, this should fix the error. Now when you run the ` ruff check ./src`, you should see:
```
root@controlplane ~/code/fraud-detection via 🐍 v3.12.3 ➜  ruff check ./src
All checks passed!
```

- Now, check the same with `black`. Use `black --check src/` and if everything is fine, you should
see:

```
root@controlplane ~/code/fraud-detection via 🐍 v3.12.3 ➜  black --check ./src
All done! ✨ 🍰 ✨
5 files would be left unchanged.
```
