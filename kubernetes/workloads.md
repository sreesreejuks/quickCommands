# Kubernetes — Workloads

## Pods

List pods in a namespace:
```bash
kubectl get pod -n <namespace>
```

Watch pods in a namespace live, e.g. right after installing/upgrading a
release, until they reach `Running`/`Ready`:
```bash
kubectl get pod -n <namespace> --watch
```

Poll pod status in a namespace on a fixed interval, clearing the screen each
time — an alternative to `--watch` for longer-running situations (e.g.
monitoring a hotfix rollout) where a clean refreshed snapshot is easier to
read than a scrolling stream of changes:
```bash
watch -n <interval-seconds> "kubectl get pods -n <namespace>"
```

Describe a pod (check status, events, recent errors):
```bash
kubectl describe pod -n <namespace> <pod-name>
```

Delete a pod (it will be recreated automatically if it belongs to a
Deployment — this is a common way to force a single pod to restart):
```bash
kubectl delete pod <pod-name> -n <namespace>
```

Force-delete a pod that's stuck in `Terminating` and won't go away normally:
```bash
kubectl delete pod <pod-name> -n <namespace> --grace-period=0 --force
```

Forward a local port to a pod, so you can reach it directly from your machine
(e.g. to hit a debug/health endpoint without going through a Service):
```bash
kubectl port-forward -n <namespace> pod/<pod-name> <local-port>:<pod-port>
```

Copy a file out of (or into) a pod:
```bash
kubectl cp <namespace>/<pod-name>:<path-in-pod> <local-path>
```

## Deployments

A Deployment manages a set of pods for you (how many replicas, which image
version, etc.). These commands act on the Deployment, not an individual pod.

Restart all pods in a Deployment one by one (rolling restart, no downtime) —
useful when a pod is misbehaving or you need it to pick up a new
ConfigMap/Secret without changing the image:
```bash
kubectl rollout restart deployment <deployment-name> -n <namespace>
```

Watch the status of an ongoing rollout (e.g. after a restart or a new
deploy) until it finishes:
```bash
kubectl rollout status deployment <deployment-name> -n <namespace>
```

Roll back a Deployment to its previous version (e.g. a bad release):
```bash
kubectl rollout undo deployment <deployment-name> -n <namespace>
```

Manually change the number of running replicas:
```bash
kubectl scale deployment <deployment-name> --replicas=<count> -n <namespace>
```

## Autoscaling (HPA)

List HorizontalPodAutoscalers in a namespace and see current vs. target
metrics at a glance:
```bash
kubectl get hpa -n <namespace>
```

Show why an HPA is (or isn't) scaling — check the `Conditions` block
(`AbleToScale`/`ScalingActive`/`ScalingLimited`) and `Events` for the
controller's actual reasoning, not just the current metric snapshot:
```bash
kubectl describe hpa <hpa-name> -n <namespace>
```
If replicas look "stuck" despite metrics sitting under target, it's often
not a bug: HPA only acts outside a ±10% tolerance band around target, and
then computes `ceil(currentReplicas × (currentMetric / targetMetric))` —
so once at 2 replicas, the metric must drop below roughly half the target
before it recommends scaling back to 1. Check `Conditions`/`Events` before
assuming something's broken.

## Exec / Debug

Open a shell inside the ArgoCD application controller pod (`<argocd-namespace>`
is usually `argocd`; the controller runs as a StatefulSet, so its pod is
almost always ordinal `-0`):
```bash
kubectl exec -it -n <argocd-namespace> <controller-pod-name> -- bash
```
