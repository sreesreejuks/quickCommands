# Kubernetes — Observability

## Events

Events are the cluster's log of what happened to resources (scheduling
failures, image pull errors, crashes, etc.). This is often the fastest way to
see what's wrong — check this before diving into `describe pod`:
```bash
kubectl get events -n <namespace> --sort-by='.lastTimestamp'
```

## Logs

Tail the last 200 lines of a pod's logs:
```bash
kubectl logs <pod-name> -n <namespace> --tail 200
```

Save the previous (pre-restart/crash) container's logs to a file:
```bash
kubectl logs -n <namespace> <pod-name> --previous > <pod-name>.pod.log
```

## Resource Usage

Show live CPU/memory usage per pod in a namespace (helpful for spotting
what's about to hit its resource limits):
```bash
kubectl top pod -n <namespace>
```

Show live CPU/memory usage per node in the cluster:
```bash
kubectl top node
```
