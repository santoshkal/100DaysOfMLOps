# task: Create a Standard ML Project Structure

A colleague has started a new ML project at /root/code/fraud-detection/, but the layout does not match the xFusionCorp Industries standard. Bring the project in line with the team's conventions.


Inspect the existing project at /root/code/fraud-detection/.

The final layout must match the tree below exactly:

```
fraud-detection/
├── data/
│   ├── raw/
│   └── processed/
├── models/
├── notebooks/
├── src/
│   ├── data/
│   ├── features/
│   ├── models/
│   └── utils/
├── tests/
├── configs/
├── requirements.txt
└── README.md
```


Every subdirectory under src/ must contain an __init__.py file so that Python recognises it as a package.

requirements.txt must list the following dependencies, one per line: scikit-learn, pandas, numpy, and mlflow. The canonical PyPI name for the scikit-learn package is scikit-learn.

README.md must begin with the heading # fraud-detection.

Review the existing project and correct everything that does not match the requirements above.

---

# Solution:

- First change into the `fraud-detection` directory and check `requirements.txt`. Update it according to the task requirements. It should look like this:

> **NOTE**: There are two `README.md` files in this task. One is at `./code` and the one we will work on is at `./code/fraud-detection`. First `cd` into the `fraud-detection` directory.


```
scikit-learn
pandas
numpy
mlflow
```

Next, update the folder structure as defined in the task.
**Note**: Some directories under `./fraud-detection/src` are currently singular (e.g., `util`, `feature`). These should be plural as specified in the task requirements.

- Update the heading in `README.md`.

