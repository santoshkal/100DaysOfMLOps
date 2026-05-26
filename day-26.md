# Task: Compare Model Runs and Select the Best


A xFusionCorp Industries data scientist has trained three candidate models on the same problem and logged them to the `model-comparison` experiment. Your task is to review the candidates side by side in the MLflow UI and explicitly mark the winning run so downstream tooling can pick it up.


1. The MLflow tracking server is already running on port `5000` and the `model-comparison` experiment has been pre-populated with three runs, each named after its algorithm (`RandomForest`, `GradientBoosting`, `LogisticRegression`) and carrying `accuracy` and `f1_score metrics`. The runs can be viewed via the MLflow UI button → `model-comparison` experiment.

2. Using the MLflow UI, inspect the three runs side by side and identify the winner by `metrics.f1_score`.

  - The run with the highest `f1_score` must carry a run-level tag: key `production-candidate`, value `true`.
  - Neither of the other two runs may carry a production-candidate tag.
> 
> The result can be confirmed in the MLflow UI: the model-comparison experiment lists three runs, and only the top-f1_score run shows the production-candidate tag on its detail page.


---

# Solution:

- Navigate to the MLflow UI and select the `model-comparison` experiment.

![select-experiment](./assets/mlops-day26.png)

- Inspect the training runs within the experiment:

![runs](./assets/mlops-day26-1.png)

- The task is to compare the runs side by side and select the one with the highest `metrics.f1_score`.

![filter-by-f1-score](./assets/mlops-day26-2.png)

- Once we identify the run with the highest `f1_score`, tag it with `production-candidate: true`.

Select the run with the highest score and add the tag.

![add-tags](./assets/mlops-day26-3.png)

![add-tags1](./assets/mlops-day26.4.png)

- Verify that no other runs carry the `production-candidate` tag:

![verify-tag](./assets/mlops-day26-5.png)

Hit **Check**



