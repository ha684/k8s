# Lab 02: kubectl Basics and Debugging

**Goal:** get comfortable with the commands you'll use every day. Run each command and read the output before moving on.

**Prerequisite:** Lab 01 checkpoint passes.

## Part 1: Create, inspect, scale

```bat
kubectl create namespace lab02
kubectl config set-context --current --namespace=lab02
kubectl create deployment web --image=nginx:1.27 --replicas=2
kubectl get deployments
kubectl get pods -o wide
kubectl get all
```

Now scale and watch:

```bat
kubectl scale deployment web --replicas=4
kubectl get pods -w
```

Press `Ctrl+C` to stop watching.

## Part 2: Break something on purpose

Delete a Pod and watch the Deployment replace it:

```bat
kubectl delete pod <one-pod-name>
kubectl get pods
```

Then try a bad image:

```bat
kubectl set image deployment/web nginx=nginx:does-not-exist
kubectl get pods
kubectl describe pod <a-new-pod-name>
```

Find the error in the `Events` section at the bottom of `describe`.

Fix it:

```bat
kubectl rollout undo deployment/web
kubectl rollout status deployment/web
kubectl rollout history deployment/web
```

## Part 3: Logs, exec, and port-forward

```bat
kubectl logs deployment/web
kubectl exec -it deploy/web -- sh
```

Inside the container, run `ls /usr/share/nginx/html` and `exit`.

Forward a local port:

```bat
kubectl port-forward deployment/web 8080:80
```

Open http://localhost:8080 in your browser. Press `Ctrl+C` in the terminal to stop.

## Part 4: Events and cleanup

```bat
kubectl get events --sort-by=.lastTimestamp
kubectl delete namespace lab02
```

## Checkpoint

Without looking at notes, write the commands to:
1. Show why a Pod is not starting
2. Roll back a bad deployment
3. Open a shell inside a running container

<details>
<summary>Show answer</summary>

1. `kubectl describe pod <name>` (read the Events) and `kubectl logs <name>` for application errors. For a crashing container, add `--previous`.
2. `kubectl rollout undo deployment/<name>`
3. `kubectl exec -it <pod-name> -- sh` (or `bash` if the image has it)

</details>

<details>
<summary>Show answer (common mistakes)</summary>

- Using `kubectl exec` on a Deployment name works, but it picks one Pod. Use `kubectl get pods` to target a specific one.
- `kubectl set image` changes the Deployment, not the Pod. Pods are replaced, which is why a bad image produces new failing Pods.
- Forgetting the namespace: if a resource "disappeared", check `kubectl get all -A`.

</details>
