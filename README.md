# 100 Days of MLOps — Hands-On Lab Series

[![KodeKloud](https://img.shields.io/badge/KodeKloud-100DaysOfMLOps-blue)](https://engineer.kodekloud.com/practice)

A comprehensive, **100-day hands-on MLOps curriculum** from **[KodeKloud](https://engineer.kodekloud.com/practice)** that takes you from foundational Python environment setup all the way to production-grade model serving, monitoring, and orchestration on Kubernetes.

Each day is a single, self-contained task that builds on an ongoing `fraud-detection` use case at the fictional **xFusionCorp Industries**. You'll encounter realistic bugs, misconfigurations, and wiring issues—just like in a real production environment—and fix them using industry-standard tools.

---

## 📚 Curriculum Overview

The 100 days are organized into **10 thematic phases**, each covering a critical layer of the MLOps lifecycle:

| Phase | Days | Topic | Description |
|-------|------|-------|-------------|
| 🏗️ **Foundation** | 1–9 | Project Setup & Tooling | Python envs, project structure, Makefiles, code quality, packaging |
| 📦 **Data Versioning** | 10–19 | DVC | Data/ML pipeline versioning, remote storage, experiments |
| 🔬 **Experiment Tracking** | 20–29 | MLflow Tracking | Logging, comparing, registering, and promoting ML models |
| 🚀 **Production Pipelines** | 30–39 | Advanced MLflow + Tuning | HPO with Optuna, PostgreSQL/S3 backends, multi-model pipelines |
| 🗃️ **Feature Stores & Secrets** | 40–49 | Feast + Vault + Docker | Feature definitions, secure secrets management, Docker packaging |
| 🐳 **Containerization** | 50–59 | Docker for ML | Multi-stage builds, health checks, Compose stacks, security |
| 🌐 **Model Serving** | 60–69 | Deployment Strategies | BentoML, A/B testing, canary deploys, Kubernetes basics |
| 📊 **Monitoring** | 70–79 | Observability | Prometheus, Grafana, Evidently drift detection, alerting |
| 🔄 **CI/CD & GitOps** | 80–89 | Pipelines + Argo | Gitea CI/CD, reusable workflows, Argo Workflows |
| 🎯 **Capstone** | 90–100 | Advanced Orchestration | Argo CronWorkflows, Kubeflow, ArgoCD GitOps, final capstone suite |

---

## 🛠️ Technology Stack

### Core Languages & Runtimes
- **Python 3.10+** – Primary language for all ML tasks
- **Bash** – Scripting and automation
- **YAML** – Configuration for pipelines, workflows, and deployments

### Data & Experiment Tracking
| Tool | Purpose | Days |
|------|---------|------|
| [DVC](https://dvc.org/) | Data and ML pipeline versioning | 10–19 |
| [MLflow](https://mlflow.org/) | Experiment tracking, model registry | 20–39, 80, 91, 97–98 |
| [SeaweedFS](https://github.com/seaweedfs/seaweedfs) | S3-compatible object store (DVC remote, MLflow artifacts) | 12–13, 19, 33–34, 52 |

### Hyperparameter Optimization
| Tool | Purpose | Days |
|------|---------|------|
| [Optuna](https://optuna.org/) | Hyperparameter tuning integrated with MLflow | 35–36, 40 |

### Feature Store & Secrets
| Tool | Purpose | Days |
|------|---------|------|
| [Feast](https://feast.dev/) | Feature store for ML | 41–43 |
| [HashiCorp Vault](https://www.vaultproject.io/) | Secrets management for MLflow | 44–47 |

### Containerization & Orchestration
| Tool | Purpose | Days |
|------|---------|------|
| [Docker](https://www.docker.com/) | Container builds, multi-stage, health checks | 48–59 |
| [Docker Compose](https://docs.docker.com/compose/) | Multi-service ML dev stack | 49, 52 |
| [Portainer](https://www.portainer.io/) | Container management UI | 61 |
| [Kubernetes](https://kubernetes.io/) | Pods, Services, Deployments, ConfigMaps, Secrets | 66–69 |

### Model Serving
| Tool | Purpose | Days |
|------|---------|------|
| [Flask](https://flask.palletsprojects.com/) | Lightweight inference APIs | 55, 62, 64, 75 |
| [FastAPI](https://fastapi.tiangolo.com/) | Modern async model serving | 63, 97 |
| [BentoML](https://www.bentoml.com/) | ML model packaging & serving | 60 |
| [BentoML Swagger UI](https://www.bentoml.com/) | Interactive API testing | 60 |

### Workflow Orchestration
| Tool | Purpose | Days |
|------|---------|------|
| [Argo Workflows](https://argoproj.github.io/workflows/) | Kubernetes-native workflow engine | 85–94 |
| [ArgoCD](https://argoproj.github.io/cd/) | GitOps deployment | 96, 99 |
| [Kubeflow Pipelines](https://www.kubeflow.org/) | ML pipeline platform on K8s | 95 |

### Monitoring & Observability
| Tool | Purpose | Days |
|------|---------|------|
| [Prometheus](https://prometheus.io/) | Metrics collection & alerting | 70–75, 77, 100 |
| [Grafana](https://grafana.com/) | Dashboards & alerting (webhooks) | 70–75, 78, 100 |
| [Evidently AI](https://www.evidentlyai.com/) | ML model drift & quality monitoring | 70–71, 75–76, 98 |

### CI/CD & Version Control
| Tool | Purpose | Days |
|------|---------|------|
| [Git](https://git-scm.com/) | Version control for code, data, and config | All days |
| [Gitea](https://gitea.io/) | Self-hosted Git with CI/CD (Actions) | 80–84 |
| [Make](https://www.gnu.org/software/make/) | ML workflow automation | 5, 40 |
| [Cookiecutter](https://github.com/cookiecutter/cookiecutter) | Project templating | 9 |

### Code Quality
| Tool | Purpose | Days |
|------|---------|------|
| [ruff](https://docs.astral.sh/ruff/) | Fast Python linter | 6, 8 |
| [black](https://black.readthedocs.io/) | Code formatter | 6, 8 |
| [pre-commit](https://pre-commit.com/) | Git hook automation | 8 |
| [pyproject.toml](https://pip.pypa.io/en/stable/reference/build-system/pyproject.toml/) | Modern Python packaging | 6, 7 |

### Package Management
| Tool | Purpose | Days |
|------|---------|------|
| [uv](https://docs.astral.sh/uv/) | Fast Python package manager | 3 |
| [pip](https://pip.pypa.io/) | Python dependency installation | 1, 3 |

---

## 📋 Detailed Day-by-Day Breakdown

### 🏗️ Phase 1: Foundation & Project Setup (Days 1–9)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 1 | Create a Python Virtual Environment for ML | `venv`, `pip`, `requirements.txt` |
| 2 | Set Up and Configure Jupyter Notebook Server | Jupyter config, port binding, server management |
| 3 | Fix a Broken uv Lockfile Specification | `uv`, `requirements.in`, dependency pinning |
| 4 | Create a Standard ML Project Structure | Directory conventions, `__init__.py`, project layout |
| 5 | Create a Makefile for ML Workflow Automation | Make targets, `.PHONY`, task orchestration |
| 6 | Set Up Code Quality Tools for ML Code | `ruff`, `black`, `pyproject.toml` configuration |
| 7 | Package an ML Project as Installable Package | `pyproject.toml`, `setuptools`, `python -m build` |
| 8 | Configure Pre-Commit Hooks for ML Repository | `.pre-commit-config.yaml`, git hooks |
| 9 | Create a Custom ML Project Template with Cookiecutter | Jinja templating, `cookiecutter.json`, project generation |

### 📦 Phase 2: Data Version Control — DVC (Days 10–19)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 10 | Install and Initialize DVC in an ML Project | `dvc init`, `.dvc/` directory structure |
| 11 | Track a Dataset with DVC | `dvc add`, `.gitignore`, DVC pointer files |
| 12 | Configure DVC Remote Storage | `.dvc/config`, S3-compatible remotes (SeaweedFS) |
| 13 | Pull DVC-Tracked Data from Remote | `dvc pull`, authentication setup |
| 14 | Create a DVC Pipeline for Data Processing | `dvc.yaml`, stages, dependencies, outputs |
| 15 | Parameterize a DVC Pipeline | `params.yaml`, `dvc repro`, parameter dependencies |
| 16 | Track ML Metrics with DVC | `metrics` section in `dvc.yaml`, `dvc metrics show` |
| 17 | Run and Compare DVC Experiments | `dvc exp run`, `dvc exp show`, `dvc exp apply` |
| 18 | Version Datasets and Models Across Git Branches | `git tag`, `git branch`, `dvc checkout` |
| 19 | Build Complete DVC ML Pipeline (cumulative) | Full pipeline with remote storage, tagging, `dvc push` |

### 🔬 Phase 3: Experiment Tracking — MLflow (Days 20–29)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 20 | Install and Start the MLflow Tracking Server | `mlflow server`, SQLite backend, artifacts |
| 21 | Log an MLflow Experiment | `mlflow.log_params`, `log_metrics`, `log_model` (sklearn) |
| 22 | Compare Runs & Register Models via MLflow UI | UI-based run comparison, model registration |
| 23 | MLflow Autologging | `mlflow.sklearn.autolog()`, automatic tracking |
| 24 | Search MLflow Runs via SDK | `MlflowClient.search_runs()`, programmatic querying |
| 25 | Register, Version, and Manage Model Lifecycle | Model Registry, aliases (`champion`/`challenger`) |
| 26 | Model Registry Stage Promotion | `Staging` → `Production` alias promotion |
| 27 | Batch Predict with Spark UDF | `mlflow.pyfunc.spark_udf`, Spark integration |
| 28 | Export & Reimport Model Registry | Registry export/import patterns |
| 29 | Export & Reimport MLflow Runs | Run metadata migration |

### 🚀 Phase 4: Production Pipelines & Tuning (Days 30–39)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 30 | End-to-End MLflow Lifecycle | Train → Register → Serve → Monitor (full loop) |
| 31 | Config-Driven Training with YAML | YAML config, reproducible training scripts |
| 32 | MLflow with PostgreSQL Backend | Production-grade backend store |
| 33 | MLflow with S3 Artifact Store (SeaweedFS) | S3 artifacts, endpoint configuration |
| 34 | MLflow with PostgreSQL + S3 Backend | Combined production setup |
| 35 | Hyperparameter Tuning with Optuna | Optuna studies, MLflow integration |
| 36 | Automated HPO Pipeline: Optuna + MLflow | End-to-end hyperparameter optimization |
| 37 | Train Multiple Models with MLflow | Side-by-side model comparison |
| 38 | Automated Training Pipeline with Cross-Validation | CV scoring, `KFold` integration |
| 39 | MLflow with MinIO Artifact Store | Alternative S3-compatible storage |

### 🗃️ Phase 5: Feature Stores & Secrets (Days 40–49)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 40 | Production Training System (cumulative) | Validation → Tuning → Selection → Registration → Report |
| 41 | Install & Initialize Feast Feature Store | `feast init`, `feast apply`, `feast ui` |
| 42 | Define Feature Views in Feast | `FeatureView`, `Entity`, `FileSource` definitions |
| 43 | Feature Engineering Pipeline with Feast | Feature serving for training/inference |
| 44 | Secure MLflow with HashiCorp Vault | Vault KV engine, secret injection |
| 45 | Fix a Broken Vault KV Policy | Policy editing, capability management |
| 46 | Vault Dynamic DB Secrets for MLflow | Dynamic database credentials |
| 47 | Vault + MLflow Production Secrets | Production secrets architecture |
| 48 | Dockerfile Best Practices for ML | Layer optimization, dependency management |
| 49 | Create Docker Compose ML Stack | Multi-service ML environment |

### 🐳 Phase 6: Docker & Containerization (Days 50–59)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 50 | Create Docker Image for ML Training | `python:3.11-slim`, pip install, `docker build` |
| 51 | Multi-Stage Docker Build for ML Serving | Builder pattern, slim runtime images |
| 52 | Local ML Dev Environment with Docker Compose | Jupyter + MLflow + SeaweedFS Compose stack |
| 53 | CI/CD Pipeline for Docker Image | Automated image builds |
| 54 | Docker Container with GPU Support | `nvidia-docker`, GPU passthrough |
| 55 | Health Checks & Graceful Shutdown | `HEALTHCHECK`, `EXPOSE`, `docker inspect` |
| 56 | Optimize Docker Image Size for ML | Image size reduction techniques |
| 57 | Docker Security Best Practices for ML | Non-root users, image scanning |
| 58 | Docker Networking for ML Microservices | Container networking, service discovery |
| 59 | Automated Docker Image Build with CI | CI-triggered builds |

### 🌐 Phase 7: Model Serving & Deployment (Days 60–69)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 60 | Package Model as BentoML Service | BentoML serving, Swagger UI |
| 61 | Containerize ML Model API (Portainer) | Portainer UI, bind mounts, container management |
| 62 | Implement A/B Testing for Model Deployment | Traffic splitting, random routing |
| 63 | Model Serving with FastAPI | Async inference, Pydantic schemas |
| 64 | Model Serving with Flask | Lightweight REST API |
| 65 | Implement Canary Deployment | Traffic ramping, rollback thresholds |
| 66 | Kubernetes: Pods, Services, Deployments | K8s core objects for ML |
| 67 | Kubernetes: ConfigMaps & Secrets for ML | Configuration management on K8s |
| 68 | Deploy ML Model on Kubernetes | Full deployment walkthrough |
| 69 | Kubernetes: Rollout & Rollback ML Deployments | Deployment strategies on K8s |

### 📊 Phase 8: Monitoring & Observability (Days 70–79)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 70 | Evidently Test Suites + Grafana Alerts | Data quality gates, PromQL alert rules |
| 71 | Evidently Monitoring Dashboard in Grafana | Multi-panel dashboards (Time series, Stat, Bar gauge) |
| 72 | Drift Detection Alerts (Webhook) | Contact points, notification policies, severity routing |
| 73 | Prometheus Metrics Export for ML | Custom metric exposition |
| 74 | Prometheus + Grafana Monitoring Setup | Full observability stack |
| 75 | End-to-End Monitoring (Prometheus + Grafana + Evidently) | Stack wiring, troubleshooting, dashboard tags |
| 76 | Evidently AI Monitoring Dashboard | Evidently UI, drift reports |
| 77 | PromQL for ML Monitoring | Query language for model metrics |
| 78 | Grafana Dashboard for Model Performance | Performance visualization |
| 79 | Custom Metrics with Prometheus Client | Instrumentation, gauge/counter/histogram |

### 🔄 Phase 9: CI/CD & GitOps (Days 80–89)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 80 | Automate Model Registration in CI/CD (Gitea) | Repository secrets, Gitea Actions, env variables |
| 81 | Automate Model Deployment with CD Pipeline | Tag-triggered releases, OCI registry |
| 82 | End-to-End ML CI/CD Pipeline (Reusable Workflows) | `workflow_call`, composite actions |
| 83 | CI/CD for ML Model Deployment | Continuous deployment patterns |
| 84 | Container Registry for ML Artifacts | Image publishing, versioning |
| 85 | Install & Run Argo Workflows on K8s | Argo UI, hello-world workflow submission |
| 86 | Basic ML Training Workflow in Argo | `containerSet`, DAG dependencies, emptyDir |
| 87 | Argo Workflow with Artifacts & Parameters | Parameter passing, artifact passing |
| 88 | Argo Workflow with MLflow Integration | Combined Argo + MLflow pipelines |
| 89 | Argo Workflow with Parallel Steps | Fan-out/fan-in patterns |

### 🎯 Phase 10: Advanced Orchestration & Capstones (Days 90–100)

| Day | Task | Skills Practiced |
|-----|------|-----------------|
| 90 | Automated Retraining with Argo CronWorkflow | Cron schedules, suspend/resume |
| 91 | Production ML Pipeline: Argo + MLflow on K8s | Multi-bug troubleshooting, WorkflowTemplate |
| 92 | Argo Workflow with DAG Steps | Directed acyclic graph patterns |
| 93 | Advanced Argo Workflow Patterns | Loops, conditions, recursion |
| 94 | Argo Workflow with Conditional Steps | When conditions, branching |
| 95 | Kubeflow Pipelines — Install & Run Basic Pipeline | KFP UI, pipeline upload, run creation |
| 96 | GitOps Model Deployment with ArgoCD | Application creation, sync, self-heal |
| 97 | **Capstone 1/4**: End-to-End ML System | Train → Register → Serve (FastAPI) |
| 98 | **Capstone 2/4**: Monitoring & Automated Retraining | Drift detection, model re-promotion |
| 99 | **Capstone 3/4**: GitOps Deployment Loop | Gitea + ArgoCD full GitOps cycle |
| 100 | **Capstone 4/4**: Close the Loop with Observability | Prometheus + Grafana live dashboard |

---

## 🎯 Learning Outcomes

By completing this series, you will be able to:

1. **Set up reproducible Python environments** for ML development using `venv`, `uv`, and Docker
2. **Version data and ML models** with DVC, including remote storage (S3-compatible), pipelines, and experiments
3. **Track experiments systematically** with MLflow — logging parameters, metrics, artifacts, and managing the model registry
4. **Tune hyperparameters at scale** using Optuna with MLflow integration
5. **Manage feature stores** with Feast for consistent feature engineering
6. **Secure ML infrastructure** using HashiCorp Vault for secrets management
7. **Containerize ML workloads** with Docker — multi-stage builds, health checks, Compose stacks
8. **Serve models in production** via Flask, FastAPI, and BentoML with A/B and canary deployment strategies
9. **Deploy ML on Kubernetes** — Pods, Services, Deployments, ConfigMaps, Secrets
10. **Monitor models in production** with Prometheus, Grafana, and Evidently AI — drift detection, alerts, dashboards
11. **Implement CI/CD for ML** with Gitea Actions, reusable workflows, and automated model registration
12. **Orchestrate ML workflows** with Argo Workflows (DAGs, CronWorkflows, WorkflowTemplates)
13. **Adopt GitOps practices** with ArgoCD for declarative Kubernetes deployments
14. **Build end-to-end MLOps pipelines** connecting all phases of the ML lifecycle

---

## 🚀 Getting Started

Each day's task is contained in a single Markdown file (`day-<N>.md`) with:
- **Scenario description** — the problem to solve
- **Requirements** — acceptance criteria for the task
- **Solution guide** — step-by-step walkthrough with screenshots

### Prerequisites

- Basic Python knowledge
- Familiarity with Git
- Understanding of ML concepts (training, evaluation, hyperparameters)
- A KodeKloud account with access to the lab environment

### How to Use

1. Pick a day and read the scenario
2. Attempt the task in the KodeKloud lab environment
3. Refer to the solution for guidance if stuck
4. Progress sequentially — each phase builds on the previous one

---

## 📁 Repository Structure

```
100DaysOfMLOps/
├── README.md              ← You are here
├── assets/                ← Solution screenshots (350+)
├── day-1.md  to day-100.md  ← One task per file
└── .git/
```

---

## 🌐 About KodeKloud

[KodeKloud](https://engineer.kodekloud.com/practice) is a hands-on learning platform for DevOps, Cloud, and MLOps engineering. This 100 Days of MLOps program provides browser-based lab environments so you can practice every task without setting up infrastructure locally.

---

## 📄 License

This repository contains solutions and notes for the KodeKloud 100 Days of MLOps program. All rights belong to KodeKloud Learning.
