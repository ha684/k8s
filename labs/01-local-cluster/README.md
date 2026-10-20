# Lab 01: Local Cluster with Docker Desktop

**Goal:** a single-node Kubernetes cluster running on your machine.

**Time:** about 1 hour (most of it is downloading images).

## Step 1: Enable Kubernetes in Docker Desktop

1. Open Docker Desktop → **Settings** → **Kubernetes**
2. Tick **Enable Kubernetes**, then click **Apply & restart**
3. Wait until the status bar shows "Kubernetes is running" (green)

## Step 2: Point kubectl at the cluster

Docker Desktop writes a context called `docker-desktop` into your kubeconfig. In Command Prompt:

```bat
kubectl config get-contexts
kubectl config use-context docker-desktop
kubectl get nodes
```

Expected output: one node named `docker-desktop` with status `Ready`.

## Step 3: Explore the system namespace

Kubernetes runs its own components as Pods too:

```bat
kubectl get pods -n kube-system
kubectl get namespaces
```

Look at the `kube-apiserver`, `etcd`, `coredns`, and `kube-controller-manager` Pods. These are the control plane, running as containers on your node.

## Step 4: Understand what you just built

Answer these in `labs/01-local-cluster/NOTES.md`:
- Where does the control plane run in this setup?
- What is a context, and what does `kubectl` read to find the cluster?
- How is this different from `kind` or `minikube`? (Hint: you don't need to know this yet. Just note what you see.)

## Checkpoint

```bat
kubectl cluster-info
kubectl get nodes -o wide
```

Both commands return without error, and the node is `Ready`.

<details>
<summary>Show answer (troubleshooting)</summary>

- **Kubernetes stuck "Starting":** Docker Desktop → Troubleshoot → Reset Kubernetes cluster. Then Apply & restart.
- **Connection refused / unable to connect to the server:** the context is wrong or the cluster is off. Run `kubectl config use-context docker-desktop` and confirm Docker shows Kubernetes as running.
- **Pods stuck in `ImagePullBackOff`:** usually a network or proxy issue. Check Docker Desktop can pull `docker run --rm hello-world`.
- **Low memory:** Docker Desktop → Settings → Resources. Give it at least 4 GB.

</details>

<details>
<summary>Show answer (NOTES questions)</summary>

- The control plane runs in containers inside the Docker Desktop VM, under `kube-system`.
- A context is a named entry in your kubeconfig (`%USERPROFILE%\.kube\config`) that holds the cluster address and credentials. `kubectl` uses the current context.
- `kind` runs nodes as Docker containers and is often used in CI. `minikube` runs a VM or container with more add-ons. Docker Desktop's built-in cluster is simplest for daily learning.

</details>
