# Task: Register, Version, and Manage Model Lifecycle

The xFusionCorp Industries ML platform team needs two trained candidates promoted through the MLflow Model Registry so the ops side can track which model version is serving production traffic. Both runs already exist in the `fraud-detection` experiment. Your task is to register both as versions of a new `fraud-detector` model, add a model-level description, and assign `challenger` and `champion` aliases—all through the MLflow UI.


1. The MLflow tracking server is already running on port `5000` and two runs are pre-populated in the `fraud-detection` experiment: a baseline run (`n_estimators=100`, `max_depth=5`, `f1_score=0.80`) and an improved run (`n_estimators=200`, `max_depth=10`, `f1_score=0.89`). Both runs can be opened via the MLflow UI button → `fraud-detection` experiment.

2. Using the MLflow UI, reach the end state below. The order (baseline first, improved second) matters because MLflow assigns version numbers sequentially within a registered model.

  - A registered model named `fraud-detector` exists in the Model Registry.
  - The registered model carries a non-empty description that references the word `fraud` (any phrasing; for example `Fraud detection model for xFusionCorp transactions`).
  - Version 1 of `fraud-detector` is the baseline run and carries the alias `challenger`.
  - Version 2 of `fraud-detector` is the improved run and carries the alias `champion`.

> The result can be confirmed by openin``g Model registry → fraud-detector in the MLflow UI. Two versions are listed, the description is shown at the top of the model page, and the alias column (or the Aliases field on each version) indicates challenger on v1 and champion on v2.


--- 
# Solution:

- We need to register the `fraud-detector` model with two versions and give them aliases.

- Navigate to the MLFlow UI, and Select **Model registry** and click the *Create Model* button and
create a model named `fraud-detector`.

![create-model](./assets/mlops-day25.png)

![create-mode-1](./assets/mlops-day25-1.png)

- Once the model is created, expand it and enter the description.

![edit-description](./assets/mlops-day25-2.png)

- Now navigateback to **Models**, you can see two models there namely `baseline`,
  and `improved`. We need to register these model, version then, and give them aliases.

![register-model](./assets/mlops-day25-4.png)

- First select the `baseline` model and register it. Once we register it, it will get Version `v1`.

![register-version](./assets/mlops-day25-5.png)

![version](./assets/mlops-day25-5a.png)
- Do the same with `improved` model, it will be versioned `v2` when registered.

![v2](./assets/mlops-day25-6.png)

- Now navigate back to **Model registry** and select `fraud-detector`. Here, we need to provide alises to
  the moldes.

![Alias-1](./assets/mlops-day25-7a.png)

Note: You might have to refresh the browser after adding and saving a tag to a version.

Hit **Check**



