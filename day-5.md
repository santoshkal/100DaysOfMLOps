# Task: Create a Makefile for ML Workflow Automation

The xFusionCorp Industries ML team uses a Makefile to orchestrate common tasks—data processing, training, testing, and cleanup. A draft Makefile exists at /root/code/fraud-detection/Makefile, but make all does not complete successfully. Bring the Makefile in line with the team's standard.


Change into `/root/code/fraud-detection/` and run make all to observe the current failure.

The corrected `Makefile` must declare the following six targets and behaviour:

- setup – Creates a virtual environment at `mlops-venv/` and installs dependencies from `requirements.txt`;
- data – Runs `python src/data/process_data.py`;
- train – Runs `python src/models/train.py`;
- test – Runs `pytest tests/`;
- clean – Recursively removes every `__pycache__` directory, removes `.pytest_cache`, and clears the contents of `models/`;
- all – Runs `setup`, `data`, `train`, and `test` in that order.

All six target names must be declared as `.PHONY` so that Make never confuses them with files of the same name.

After your changes, `make all` must complete without error.

Makefile recipes must be indented with a real tab character, not spaces. Make rejects any recipe that is not tab-indented.


---
# Solution:

**Note**: Refer to the [GNU Make manual](https://www.gnu.org/software/make/manual/make.html#toc-An-Introduction-to-Makefiles) for more information on Makefiles.
- Change into the `fraud-detection` directory and examine the existing `Makefile`.
  - You may find inconsistencies: some rules use spaces instead of `TAB`. Makefiles require TAB characters for recipe indentation.
  - The `clean` target does not recursively remove `__pycache__`, does not remove `.pytest_cache`, and does not clear the contents of `models/`.
  - The `all` target is missing.
  - The `.PHONY` declaration is missing.

The updated `Makefile` should look like as following:

```
# fraud-detection Makefile

.PHONY: setup data train test clean all

setup:
	python3 -m venv mlops-venv && mlops-venv/bin/pip install -r requirements.txt

data:
	python src/data/process_data.py

train:
	python src/models/train.py

test:
	pytest tests/

clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	rm -rf .pytest_cache
	rm -rf models/*

all: setup data train test
```


- Once the `Makefile` is updated, run `make all` — the command should complete without errors.

