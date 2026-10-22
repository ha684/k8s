# Lab 11: MLOps, Serving a Model on Kubernetes (planned)

**Goal:** serve a trained model behind an API, the way a production MLOps pipeline would.

Planned content:
- Package a small model (for example scikit-learn) behind FastAPI in a container
- Expose it as a Deployment with probes and resource limits
- Model versioning: store the model artifact in S3, load it by version tag
- Canary rollout of a new model version
- Overview of KServe and Kubeflow, when they're worth it, and when they're overkill

**Local note:** Docker Desktop has no GPU, so use CPU models for this lab. GPU scheduling (NVIDIA device plugin, node selectors, taints) is a later topic.
