# Lab 09: Real EKS Cluster with Terraform (planned, COSTS MONEY)

**Read the cost section before starting.**

## Before you start

- **AWS Skill Builder:** it has guided labs and hosted sandboxes for some courses, and some include EKS. Check the course catalog for an EKS or Kubernetes lab before you spend money on your own account. Sandbox sessions usually have time limits, so note when yours ends.
- **Your own account:** create an AWS Budget alert first (for example $10 per month). EKS charges per hour for the control plane, plus the nodes and any load balancers. Destroy everything at the end of each session.

Planned content:
- Terraform basics with a remote state backend (S3 + DynamoDB lock)
- Create a VPC and an EKS cluster with a managed node group using the community `terraform-aws-modules/eks` module
- Connect with `aws eks update-kubeconfig`
- Deploy the lab 03 app and reach it through an AWS Load Balancer Controller Ingress
- `terraform destroy`, then verify in the console that nothing is left running
