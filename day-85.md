# Task:  Install Argo Workflows on Kubernetes

The xFusionCorp Industries ML platform team has stood up a fresh Kubernetes cluster for workflow orchestration. The cluster is running, **Argo Workflows** `v4.0.4` is installed in the `argo` namespace, both the `workflow-controller` and `argo-server` Deployments are Available, and the Argo web UI is port-forwarded to the lab. Your task is to submit the canonical `hello-world` workflow through the Argo UI and watch it complete on the Workflows list.


1. The Argo UI button at the top of the lab opens the Workflows page on port `5000`. The UI has no auth (quick-start install) — the Workflows list is empty on first open.

2. From the Argo UI:

  - Click + Submit New Workflow (top-right on the Workflows list).
  - The in-browser YAML editor opens with a default template that references an image the cluster's containerd no longer pulls. Replace the editor contents with a small hello-world using a current image (e.g. alpine:3.20) and keep metadata.namespace: argo.
  - Click + Create.

3. The UI navigates to the new workflow's detail page. The single node on the graph moves through *Pending* → *Running* → *Succeeded* (green tick). Click the node to inspect the pod's log.

4. The end state must include:

  - GET http://localhost:5000/ returns 200 (Argo UI reachable).
  - workflow-controller and argo-server Deployments in namespace argo are Available.
  - GET /api/v1/workflows/argo lists at least one workflow.
  - The most recently submitted workflow has status.phase == Succeeded (tests wait up to 180 s for it to reach a terminal phase).

> Argo's + Submit New Workflow flow is how every future lab in this section starts. The UI's YAML editor is the canonical authoring surface—not kubectl apply -f file.yaml from a terminal. Treating the UI as the primary tool means every Workflow, WorkflowTemplate, and CronWorkflow change lives under a click trail rather than a shell history.
