# Lab 03: Declarative YAML with Deployments and Services

**Goal:** stop using imperative commands. Describe the app in files, apply them, and change them by editing files.

**Files in this folder:**
- `deployment.yaml`
- `service.yaml`

## Step 1: Read the Deployment

Open `deployment.yaml`. Map each part to what you know from ECS:
- `replicas` ≈ ECS desired count
- `template.spec.containers` ≈ container definitions in a task definition
- `selector` and `labels` are how the Deployment finds its Pods. This is Kubernetes' core matching mechanism.

## Step 2: Apply

```bat
cd labs\03-first-manifests
kubectl create namespace lab03
kubectl apply -f deployment.yaml -n lab03
kubectl apply -f service.yaml -n lab03
kubectl get all -n lab03
```

## Step 3: Access it

```bat
kubectl port-forward service/hello 8080:80 -n lab03
```

Open http://localhost:8080.

## Step 4: Change it declaratively

Edit `deployment.yaml`: change `replicas` from 2 to 4. Then:

```bat
kubectl apply -f deployment.yaml -n lab03
kubectl get pods -n lab03
```

Now change the image tag from `nginx:1.27` to `nginx:1.28`, apply again, and run:

```bat
kubectl rollout status deployment/hello -n lab03
kubectl rollout history deployment/hello -n lab03
```

## Step 5: Understand `apply` vs `create`

Run `kubectl apply -f deployment.yaml -n lab03` twice in a row. The second run should say `unchanged`. Explain why in your NOTES.

## Checkpoint

1. You can change the replica count by editing YAML and running `apply`, without any imperative command.
2. `kubectl get endpoints hello -n lab03` lists the Pod IPs behind the Service.
3. You can explain in one sentence how the Service finds its Pods.

<details>
<summary>Show answer (Service selection)</summary>

The Service has a `selector` with `app: hello`. Any Pod with that label becomes an endpoint. The Deployment's Pods carry that label in `template.metadata.labels`. Labels are the link. If the labels don't match, the Service has no endpoints even though Pods are running. This is the most common first-time bug.

</details>

<details>
<summary>Show answer (Step 5)</summary>

`apply` compares the file with the live object and only sends changes. Running it again with no edits does nothing, so it reports `unchanged`. `create` always tries to make a new object and fails if it exists.

</details>

<details>
<summary>Show answer (cleanup)</summary>

`kubectl delete namespace lab03`

</details>
