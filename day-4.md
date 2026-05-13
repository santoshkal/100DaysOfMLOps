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

- First cd into the `fraud-detection` dir and check the `requirements.txt`. We need to update it
according to the requirements of the task. It should look like this:

> **NOTE**: There are two README.md in this task. One is in the root at `./code` and the one we will
> work is at `./code/fraud-detection`. First `cd` into the `fraud-detection` directory.


```
scikit-learn
pandas
numpy
mlflow
```

Next update the folder structure as defined in the question. 
**Note**: The dir names in the `./fraud-detection/src`. Some dir are currently singlar like `util`,
`feature`. These should be plural as defined in the task.

- Update the header in the README.md

