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

**Note**: Follow this manual on [more information on Makefile](https://www.gnu.org/software/make/manual/make.html#toc-An-Introduction-to-Makefiles)
- cd into the `fraud-detection` dir, and checkout the `Makefile` available.
  - You might find some inconsistencies in teh Makefile, it has used spaces instead of `TAB` in some rules. Makefiles requires TAB spacing.
  - `clean` does not recursively remove `__pycache__`, `.pytest_cache`, and `models/` contents are not cleared.
  - The `all` target is missing
  - `PHONY` decleration is missing

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


- Once the `Makefile` is updated, run `make all` and the command should return with no errors.

