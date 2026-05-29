# Task: Production Model Serving with Docker Compose


The xFusionCorp Industries ML platform team runs the fraud-detection model in production behind a full observability stack — a Flask API with *Prometheus* instrumentation, *Redis* for per-IP rate limiting, *nginx* as the public reverse proxy, *Prometheus* for metrics collection, and *Grafana* for dashboarding. The pre-staged stack at `/root/code/serving/production/` does not currently reach a clean end state. Your task is to correct the wiring, bring the stack up, and create a *Grafana* dashboard that visualises the model API's request rate.


1. The Docker daemon is already running. Every image the compose stack references is being pre-pulled in the background at startup, so the reader's first `docker compose up -d` (after applying the three fixes) returns in seconds.

2. The project layout under `/root/code/serving/production/`:

  - `app/app.py` – Flask API with `/health,` `/predict` (Redis-backed per-IP rate limit), and `/metrics` (once the exporter is wired). Needs attention.
  - `app/Dockerfile` – `python:3.11-slim` + flask + redis + prometheus-flask-exporter + joblib + sklearn. Correct.
  - `model.pkl` – Trained at startup on the shared synthetic fraud dataset.
  - `docker-compose.yml` – Defines model-api, `redis`, `nginx` (publishes `8085`), `prometheus` (publishes `9090`), `grafana` (publishes `3000`, admin password `grafana2026`), and a traffic-generator sidecar (continuously POSTs to `/predict` so `Grafana` has live request-rate data to plot). Correct.
  - `prometheus.yml` – Scrape config for the `model-api` job. Needs attention.
  - `nginx.conf` – Reverse-proxy config with an upstream model_backend block + location / forwarding every request. Needs attention.
  - `grafana/provisioning/datasources/prometheus.yml` – Pre-provisions a Prometheus datasource pointing at `http://prometheus:9090`, so the reader's Grafana task focuses on dashboard creation.

3. Fix the three wiring issues, bring the stack up with `docker compose up -d` from `/root/code/serving/production/`, then open the Grafana UI button, log in with `admin` / `grafana2026`, and create a dashboard carrying at least one panel that queries the Prometheus datasource.

4. The end state must include:

  - All six containers `(model-api`, `prod-redis,` `prod-nginx`, `prod-prometheus`, `prod-grafana`, `prod-traffic)` are reported running by docker inspect.
  - `docker exec model-api curl -s http://localhost:5000/metrics` returns a Prometheus exposition-format body (HTTP 200).
  - `curl -X POST http://localhost:8085/predict -d '{...}'` through nginx returns a JSON is_fraud response.
  - `curl http://localhost:9090/api/v1/targets` reports the model-api job's health as up.
  - `curl -u admin:grafana2026 http://localhost:3000/api/datasources` lists a Prometheus datasource.
  - `curl -u admin:grafana2026 http://localhost:3000/api/search?type=dash-db` returns at least one user-created dashboard, and that dashboard's JSON carries at least one panel.


> The Flask app listens on container port 5000. Every example Prometheus query for Flask exporters shows the flask_http_request_total counter; rate(flask_http_request_total[1m]) is a reasonable per-second request-rate panel expression.

---

# Solution:

