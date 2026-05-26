# Task: Package an ML Project as Installable Python Package

The xFusionCorp Industries deployment team needs the fraud-detection model code packaged as an installable Python distribution. A draft `pyproject.toml` exists at `/root/code/fraud-detection/`, but it does not build a wheel that meets the team's standard. Correct the file and produce a compliant package.


1. The project at /root/code/fraud-detection/ already contains the source code under src/fraud_detection/. The Python files are complete—you do not need to modify any of them.

2. The corrected pyproject.toml must satisfy every one of the following:

- it declares a `[build-system]` section with `requires = ["setuptools>=61.0", "wheel"]` and `build-backend = "setuptools.build_meta"`;
- `name` is `fraud_detection` (the distribution name must match the module path under `src/`);
- `version` is `0.1.0`;
- `requires-python` is `>=3.10`;
- `dependencies` is `["scikit-learn", "pandas", "numpy"]`.

3. Review the existing `pyproject.toml` and correct everything that does not match the requirements above.

4. Build the package from the project directory:

```
   cd /root/code/fraud-detection
   python3 -m build
```

5. The build must produce a wheel named `fraud_detection-0.1.0-*.whl` under `dist/`.

The `build` package is already installed. Use `python3` rather than `python`.

---

# Solution:

- Change into the `fraud-detection` directory and examine the `pyproject.toml`. The main task is to update this file according to the requirements. The updated `pyproject.toml` should look like this:

```
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "fraud_detection"
version = "0.1.0"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.10"
dependencies = [
    "scikit-learn",
    "pandas",
    "numpy"
]

[tool.setuptools.packages.find]
where = ["src"]
```

- Once updated, run `python3 -m build` and the wheel file should be built and written into the `./dist` directory.
  
