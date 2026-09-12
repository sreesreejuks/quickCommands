# Kubernetes — Networking

## Services & Ingress

List Services in a namespace (a Service is the stable network address that
sits in front of a set of pods):
```bash
kubectl get svc -n <namespace>
```

List Ingresses in a namespace (Ingress is what routes external HTTP(S)
traffic into a Service):
```bash
kubectl get ingress -n <namespace>
```
