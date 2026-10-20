# Lab 10: CI/CD with GitHub Actions (planned)

**Goal:** build an image on every push, push it to ECR, and deploy it to your cluster.

Planned content:
- Build and tag an image with the git SHA
- Authenticate to AWS with GitHub OIDC (no long-lived access keys)
- Push to ECR and update the Deployment image
- Rollback by reverting a commit

You already know GitHub Actions, so this lab focuses on the Kubernetes and OIDC parts.
