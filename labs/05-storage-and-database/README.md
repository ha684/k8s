# Lab 05: Volumes, PVCs, and PostgreSQL StatefulSet (planned)

**Goal:** run a stateful database on Kubernetes and understand how data outlives Pods.

Planned content:
- emptyDir vs PersistentVolume vs PersistentVolumeClaim, and how Docker Desktop's default StorageClass provisions storage
- Deploy PostgreSQL as a StatefulSet with a volumeClaimTemplate
- Connect a backend Pod to it through a Service name
- Delete the database Pod and prove the data is still there
- Why StatefulSet instead of Deployment for databases

Bridge to AWS: this maps to EBS-backed volumes (EBS CSI driver) on EKS in lab 09.
