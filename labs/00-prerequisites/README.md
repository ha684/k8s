# Lab 00: Prerequisites and ECS vs Kubernetes

**Goal:** confirm your tools work and understand where Kubernetes fits next to what you already know from AWS.

## Part 1: Check your tools

Open **Command Prompt** and run:

```bat
docker version
docker run --rm hello-world
```

You should see "Hello from Docker!". If Docker says the daemon is not running, start Docker Desktop and wait until the whale icon is steady.

Install kubectl if you don't have it. Easiest options on Windows:

```bat
winget install -e --id Kubernetes.kubectl
```

Or, if you use Chocolatey: `choco install kubernetes-cli`. Then open a **new** Command Prompt window and run:

```bat
kubectl version --client
```

Also install and verify the AWS CLI and Terraform, since later labs need them:

```bat
aws --version
terraform -version
```

<details>
<summary>Show answer (if a tool is missing)</summary>

- `winget` not found: update App Installer from the Microsoft Store.
- `kubectl` not found after install: close the terminal and open a new one so PATH refreshes.
- `docker` cannot connect: Docker Desktop must be running. Check Settings → General → "Use the WSL 2 based engine" is enabled.

</details>

## Part 2: ECS vs Kubernetes (you asked for this one)

Kubernetes (K8s) and ECS both answer the same question: "I have containers, run them on a fleet of machines, keep them healthy, and route traffic to them." They differ in scope and vocabulary.

| You know (ECS) | Kubernetes equivalent | What it means |
|---|---|---|
| Cluster | Cluster | A set of machines plus a control plane |
| Task Definition | Pod spec | The description of one or more containers that run together |
| Task | Pod | One running instance of that description |
| Service (ECS) | Deployment + Service | Deployment keeps N copies running; Service gives them a stable address |
| Container instance (EC2 launch type) | Node | The machine that runs Pods |
| Fargate | Node-less options (EKS Fargate, Karpenter, or a managed node group) | You don't manage the machine |
| ALB target group | Ingress (with a controller) | HTTP routing into the cluster |
| Service Discovery (Cloud Map) | Cluster DNS | Pods find each other by Service name |
| Secrets Manager reference | Secret | Sensitive config injected into a container |
| Task role (IAM) | ServiceAccount + IRSA (EKS) | Per-workload AWS permissions |

Key differences to remember:
- **Desired state with reconciliation:** you declare YAML, and a controller loop keeps reality matching it. ECS does this too, but Kubernetes applies the same model to everything: networking, storage, config, scaling.
- **Portability:** the same YAML runs on Docker Desktop, kind, EKS, GKE, or AKS. ECS is AWS-only.
- **Ecosystem:** Helm, Argo CD, Kubeflow, KServe, and most MLOps tools assume Kubernetes. That's the main reason it fits your MLOps goal.
- **More moving parts:** Kubernetes has more to learn, and you operate more of it. EKS manages the control plane for you; you still manage add-ons, upgrades, and node groups.

Rule of thumb: ECS is simpler when everything is on AWS and you don't need the ecosystem. Kubernetes is worth it for MLOps tooling and portability.

## Checkpoint

You can answer each of these without looking:
1. What is a Pod, and how is it like an ECS task?
2. Why does Kubernetes use a Deployment on top of Pods?
3. What does "desired state" mean?

<details>
<summary>Show answer</summary>

1. A Pod is the smallest deployable unit: one or more containers sharing network and storage. It's the closest equivalent to a running ECS task.
2. A Deployment manages a set of identical Pods, replaces them during rollouts, and rolls back if needed. A bare Pod is not rescheduled if its node dies.
3. You declare the state you want (for example, "3 replicas of this image"). Controllers continuously compare that with reality and act to close the gap.

</details>
