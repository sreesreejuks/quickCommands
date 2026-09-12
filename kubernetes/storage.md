# Kubernetes — Storage & Secrets

## Secrets

List secrets in a namespace:
```bash
kubectl get secret -n <namespace>
```

Describe a secret to see its key names without revealing the values:
```bash
kubectl describe secret <secret-name> -n <namespace>
```

Decode a specific value out of a secret (e.g. a password a Helm chart
generated for you on install):
```bash
kubectl get secret <secret-name> -n <namespace> -o jsonpath="{.data.<key-name>}" | base64 -d
```

Force the External Secrets Operator to immediately re-sync an
`ExternalSecret` (`es`) from its backing store, instead of waiting for its
next poll interval — annotating with a fresh value (e.g. the current epoch
timestamp) each time is required, since the operator only reacts to a
*change* in the annotation's value, not just its presence:
```bash
kubectl annotate externalsecret <externalsecret-name> -n <namespace> force-sync=<unique-value> --overwrite
```

## Persistent Volumes & Claims

List Persistent Volume Claims (PVCs) in a namespace:
```bash
kubectl get pvc -n <namespace>
```

List Persistent Volumes (PVs) — cluster-scoped, not namespaced:
```bash
kubectl get pv
```

Delete a PVC. Helm charts backed by a database (e.g. Bitnami's
WordPress/MariaDB chart) intentionally leave PVCs behind on `helm uninstall`
so the data survives a reinstall. But if the chart also auto-generates a
random password on each install, a reinstall against that old PVC will fail:
the database's existing data still expects the *old* password, while the
freshly installed release only knows the newly generated one. Delete the PVC
first if you want a truly clean reinstall:
```bash
kubectl delete pvc <pvc-name> -n <namespace>
```
