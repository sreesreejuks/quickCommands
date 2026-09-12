# Kubernetes — ArgoCD

`<argocd-namespace>` is usually `argocd`, but check your install if it was
deployed into something else.

## ArgoCD Repo-Server Debugging

Check how many repo-server replicas exist (matters when tailing logs — a
failing sync might land on a different pod than the one you're watching):
```bash
kubectl get pods -n <argocd-namespace> -l app.kubernetes.io/name=argocd-repo-server
```

Tail repo-server logs live, no filter — trigger a sync/refresh in another
terminal right after starting this to catch the failing request as it
happens, rather than guessing a `--since` window:
```bash
kubectl logs -n <argocd-namespace> <repo-server-pod-name> -f
```

Bump repo-server logging to debug level when the default `info` logs don't
show enough detail (e.g. git/auth exchanges) — pod restarts automatically
to pick this up:
```bash
kubectl set env -n <argocd-namespace> deploy/argocd-repo-server ARGOCD_REPO_SERVER_LOGLEVEL=debug
```

Revert the debug log level afterward so it doesn't stay noisy:
```bash
kubectl set env -n <argocd-namespace> deploy/argocd-repo-server ARGOCD_REPO_SERVER_LOGLEVEL-
```

List ArgoCD's stored git credentials — `repository` is per-repo entries,
`repo-creds` is wildcard/pattern-based (e.g. one credential covering a
whole org's URL prefix):
```bash
kubectl get secrets -n <argocd-namespace> -l argocd.argoproj.io/secret-type=repository
kubectl get secrets -n <argocd-namespace> -l argocd.argoproj.io/secret-type=repo-creds
```

Decode a repo-creds secret's `url`/`username` to see what it covers and
what identity it authenticates as (skip `password` — don't dump tokens to
a terminal you might paste elsewhere):
```bash
kubectl get secret <secret-name> -n <argocd-namespace> -o jsonpath='{.data.url}{"\n"}{.data.username}' | base64 -d
```

## ArgoCD CLI

These use the `argocd` CLI (not `kubectl`) and assume you're logged in
(`argocd login <server>`). ArgoCD is what's actually deploying/syncing your
workloads onto the cluster, so these are often more useful than `kubectl`
when a deployment looks wrong.

List all ArgoCD applications and their sync/health status:
```bash
argocd app list
```

Manually trigger a sync (deploy the latest committed manifests) for an app:
```bash
argocd app sync <app-name>
```

Show the difference between what's running and what's in Git for an app
(useful for spotting manual/out-of-band changes):
```bash
argocd app diff <app-name>
```

Force a hard refresh (re-fetch from git, bypass ArgoCD's manifest cache) —
use this after fixing a repo-access/credential issue instead of waiting for
the next poll cycle:
```bash
argocd app get <app-name> --hard-refresh
```

Test one repo's stored credential directly, independent of any Application
— isolates repo-creds/auth problems from sync/manifest-generation problems:
```bash
argocd repo get <repo-url>
```
