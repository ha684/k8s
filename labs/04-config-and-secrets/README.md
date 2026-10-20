# Lab 04: ConfigMaps and Secrets

**Goal:** separate configuration from the image. Inject config as environment variables and as files, and handle a secret without hard-coding it.

**Files in this folder:**
- `configmap.yaml`
- `secret.yaml`
- `pod-config-demo.yaml`

**Note:** Secrets here are base64-encoded, not encrypted. Never commit real secret values to git. The values in this lab are throwaway.

## Step 1: Create the objects

```bat
cd labs\04-config-and-secrets
kubectl create namespace lab04
kubectl apply -f configmap.yaml -n lab04
kubectl apply -f secret.yaml -n lab04
kubectl apply -f pod-config-demo.yaml -n lab04
```

## Step 2: Check what the Pod sees

```bat
kubectl logs config-demo -n lab04
```

The output shows environment variables and the mounted file.

## Step 3: Change config without rebuilding the image

Edit `configmap.yaml`, change `GREETING` to something else, then:

```bat
kubectl apply -f configmap.yaml -n lab04
kubectl delete pod config-demo -n lab04
kubectl apply -f pod-config-demo.yaml -n lab04
kubectl logs config-demo -n lab04
```

Question for NOTES: why did you have to delete and recreate the Pod? (Environment variables are read at container start. Mounted files do update, but the app may not reread them.)

## Step 4: Inspect the secret

```bat
kubectl get secret app-secret -n lab04 -o yaml
```

The value looks unreadable. Decode it in Command Prompt is awkward, so use PowerShell:

```powershell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("<paste-value>"))
```

## Checkpoint

1. `kubectl logs config-demo -n lab04` shows the new `GREETING` after your edit.
2. You can explain why ConfigMap is for config and Secret is for sensitive values, even though both are just key-value data.
3. You can name one way Secrets are better than plain environment variables in a manifest.

<details>
<summary>Show answer (Step 3)</summary>

Environment variables are injected once when the container starts. Changing a ConfigMap does not change running containers' environment. Mounted ConfigMap volumes do refresh on disk after a delay (usually within a minute), but applications often cache config at startup, so restarting the Pod is the reliable way.

</details>

<details>
<summary>Show answer (checkpoint 2 and 3)</summary>

- ConfigMap is for non-sensitive settings. Secret is for passwords, tokens, and keys. Kubernetes treats Secrets differently: they can be encrypted at rest, restricted by RBAC, and kept out of some logs.
- Better than plain env vars in a manifest: the secret value is not in your YAML or git history, and it can come from an external store such as AWS Secrets Manager via External Secrets Operator. (You'll see that in later labs.)

</details>

<details>
<summary>Show answer (cleanup)</summary>

`kubectl delete namespace lab04`

</details>
