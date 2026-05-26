# Task: Add Health Checks and Graceful Shutdown to ML Containers

The xFusionCorp Industries ML platform team ships Flask-based inference APIs as Docker images with Docker-native `HEALTHCHECK` instructions so operators can read `docker inspect --format='{{.State.Health.Status}}'` and tell at a glance whether the process is serving. The draft *Dockerfile* at `/root/code/ml-health/` does not meet that bar — `docker inspect --format='{{.State.Health.Status}}' ml-health-api` reads unhealthy, and `docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1` reports no ports. Your task is to correct the `HEALTHCHECK` target and add the missing `EXPOSE` declaration.


1. The Docker daemon is already running. `docker version` can be run in a VS Code terminal to confirm.

2. The project layout under `/root/code/ml-health/`:

  - `app.py` – Flask API serving `GET /health` (returns `{"status": "ok"} / 200`) and `POST /predict` (returns a rule-based fraud flag) on port `8085`. Correct.
  - `Dockerfile` – `python:3.11-slim`, installs `flask`, copies `app.py`, carries a `HEALTHCHECK` + `CMD`.

3. Open `Dockerfile` in the VS Code editor, correct the Dockerfile, save, and from `/root/code/ml-health/`:

```
   docker build -t ml-health:v1 .
   docker run -d --rm --name ml-health-api -p 8085:8085 ml-health:v1
```


4. The end state must include:

  - The `HEALTHCHECK` probe targets `/health` (the path the Flask app actually serves).
  - The Dockerfile declares `EXPOSE 8085`, so `docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1` reports `map[8085/tcp:{}]`.
  - After `docker run`, `docker inspect --format='{{.State.Health.Status}}' ml-health-api` reads healthy within ~15 seconds.
  - `curl http://localhost:8085/health` returns `{"status": "ok"}` with HTTP 200.


> `HEALTHCHECK` reruns its `CMD` every `--interval` seconds and flips the state to unhealthy after `--retries` consecutive failures. `EXPOSE` does not change networking (that is done by -p)—it writes image metadata so `docker inspect` and downstream orchestrators know which port the image intends to serve on.

---
# Solution:


- The task is to fix the `HEALTHCHECK` instruction in the existing Dockerfile so that the health check returns `healthy` from the container.
Change into the `./ml-health` directory and inspect the Dockerfile.
- The lab requires `docker inspect` to return `healthy` within 15 seconds. The `--timeout` is set to `3s`, which is lower than the `--interval`. Increase `--timeout` to `15s`.
- The `HEALTHCHECK` CMD targets `http://localhost:8085/healthz`, but `./app.py` exposes the endpoint at `/health` on port 8085. Update the endpoint accordingly.
- The Dockerfile does not declare `EXPOSE 8085`. Add the `EXPOSE` instruction with port `8085`.

![update-Dockerfile](./assets/mlops-day55.png)

- Build the image with `docker build -t ml-health:v1 .`:

![build-image](./assets/mlops-day66.png)

- In a separate terminal, query the `/health` endpoint from the running container and verify `docker inspect` shows the expected health status:

![check-health](./assets/mlops-day55c.png)

Hit **Check**



