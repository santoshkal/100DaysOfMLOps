# Task: Set Up Local ML Dev Environment with Docker Compose

The xFusionCorp Industries ML platform team ships a local dev stack — Jupyter Lab for notebooks, MLflow for experiment tracking, SeaweedFS for S3-compatible artefact storage — as a three-service docker `compose deployment`. A draft `docker-compose.yml` exists at `/root/code/ml-dev/`, but as it ships the stack does not bring all three browser UIs up on their standard ports. Your task is to correct the `docker-compose.yml` so every service is reachable on its standard port without login prompts, bring the stack up with `docker compose up -d`, and confirm via the browser-UI buttons at the top of the lab.


1. The Docker daemon is already running and every image has been pre-pulled in the background at startup, so `docker compose up -d` returns in seconds on the first run.

2. The project layout under `/root/code/ml-dev/`:

  - `docker-compose.yml` – Three services:
    - jupyter – Container `ml-jupyter` on `jupyter/base-notebook:python-3.11`, host port `8888`.
    - mlflow – Container `ml-mlflow` on `ghcr.io/mlflow/mlflow:v2.15.1`, host port `5000`. Correct.
    - seaweedfs – Container `ml-seaweedfs` on `chrislusf/seaweedfs:4.22`. SeaweedFS serves the S3 API on container port `8333` and the Filer UI on container port `8888`. The lab's convention is host port `9000` for the S3 API and host port `9001` for the Filer UI.

3. Open `docker-compose.yml` in the VS Code editor, align it with the end state below, save, and run `docker compose up -d` from inside `/root/code/ml-dev/`.

4. The end state must include:

  - All three containers `(ml-jupyter`, `ml-mlflow`, `ml-seaweedfs`) reported Up by docker compose ps.
  - curl `http://localhost:8888/` returns 200 or 302 – The Jupyter UI answers without prompting for a token.
  - curl `http://localhost:5000/` returns 200 – The MLflow UI answers on the standard port.
  - curl `http://localhost:9001/` returns 200, 302, or 403 – The SeaweedFS Filer UI answers on its standard host port (the SeaweedFS S3 API stays on host 9000).


> The three browser UIs (Jupyter, MLflow, SeaweedFS Filer) are the primary verification surface — open them from the buttons at the top of the lab.


---

# Solution:

- The task is to fix issues in the Docker Compose file. Change into the `ml-dev` directory and inspect `docker-compose.yml` in VSCode.
    - First, check if all ports are wired correctly. The `jupyter` and `mlflow` services have correct port definitions. However, there is a mix-up in the `seaweedfs` service: the S3 API port and the Filer UI port are swapped. Fix that:

![check-ports](./assets/mlops-day52.png)


- Next, ensure the Jupyter UI answers without prompting for a token. By default, the container's entrypoint starts Jupyter with a pre-generated auth token, making the UI unreachable through the browser.

To find the correct configuration, exec into the `jupyter/base-notebook:python-3.11` image and run `start-notebook.py --help-all`. The `IdentityProvider.token` option accepts a string and can be set to an empty string `""`. Update the `jupyter` service in Docker Compose with this configuration and run `docker compose up -d` to verify all containers are up:

![update-command-and-build](./assets/mlops-day52a.png)

- Once containers are up, verify they are responding on their respective ports with `curl`:

  - `curl http://localhost:8888/` may respond with `403` — this indicates the server is running and the request reached the container.
  - `curl --head http://localhost:5000/` should return `status: 200`.
  - `curl http://localhost:9001/` should return `status: 200`.


Done, hit **Check**


