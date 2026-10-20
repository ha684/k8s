# Kubernetes Lab Guide (for an AWS developer, MLOps focus)

A hands-on path from zero to running a full-stack app on Kubernetes, with an MLOps track at the end.

## Your profile (what this guide is tailored to)

- Shell: Command Prompt on Windows (commands below are shown for `cmd`, with notes where bash/WSL differs)
- Docker Desktop installed, editor is VS Code
- Strong on AWS basics (EC2, Lambda, DynamoDB), some ECS exposure, no deep hands-on
- Knows GitHub Actions; knows Terraform conceptually, no hands-on yet
- Goal: MLOps. Final project: a full-stack app with a model-serving component
- Time: about 1 hour every night
- Certification: none yet

## How to use this guide

1. Work through the labs in order. Each lab folder has a `README.md`.
2. Every lab ends with a **Checkpoint**: a command that proves you did it right.
3. Answers are hidden in collapsible blocks. Try first, then expand. Click "Show answer" in the rendered markdown (VS Code preview: `Ctrl+Shift+V`).
4. Keep a short note of anything that surprised you in `labs/<lab>/NOTES.md`. Future you will thank you.

## Roadmap

| Lab | Topic | Est. nights | Status |
|---|---|---|---|
| [00](labs/00-prerequisites/README.md) | Prerequisites and ECS vs Kubernetes | 1 | Written |
| [01](labs/01-local-cluster/README.md) | Local cluster with Docker Desktop, kubectl | 1 | Written |
| [02](labs/02-kubectl-basics/README.md) | kubectl imperative basics and debugging | 2 | Written |
| [03](labs/03-first-manifests/README.md) | Declarative YAML: Deployments and Services | 2 | Written |
| [04](labs/04-config-and-secrets/README.md) | ConfigMaps and Secrets | 1 | Written |
| [05](labs/05-storage-and-database/README.md) | Volumes, PVCs, PostgreSQL StatefulSet | 2 | Planned |
| [06](labs/06-probes-resources-scaling/README.md) | Probes, resources, HPA | 2 | Planned |
| [07](labs/07-ingress-and-tls/README.md) | Ingress controller and routing | 2 | Planned |
| [08](labs/08-helm-and-kustomize/README.md) | Helm charts and Kustomize overlays | 3 | Planned |
| [09](labs/09-eks-with-terraform/README.md) | Real EKS cluster with Terraform (cost warning) | 4 | Planned |
| [10](labs/10-ci-cd-github-actions/README.md) | Build, push to ECR, deploy with GitHub Actions | 3 | Planned |
| [11](labs/11-mlops-model-serving/README.md) | Serving a model on Kubernetes | 4 | Planned |
| [12](labs/12-capstone-fullstack/README.md) | Capstone: full-stack app plus model on EKS | 5 | Planned |

Roughly 30 nights if you do one lab per night and repeat hard ones.

## Progress tracker

- [ ] Lab 00 · [ ] Lab 01 · [ ] Lab 02 · [ ] Lab 03 · [ ] Lab 04
- [ ] Lab 05 · [ ] Lab 06 · [ ] Lab 07 · [ ] Lab 08
- [ ] Lab 09 · [ ] Lab 10 · [ ] Lab 11 · [ ] Lab 12

## Cost warning

Labs 00–08 run entirely on your machine and cost nothing. Lab 09 and later create real AWS resources (EKS control plane is billed hourly, plus worker nodes and load balancers). Before lab 09:
- Create an AWS Budget alert (for example at $10 per month)
- Run `terraform destroy` at the end of every session
