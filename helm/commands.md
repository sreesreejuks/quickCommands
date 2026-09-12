# Helm Quick Commands

## Repos

Add a chart repo:
```bash
helm repo add <repo-name> <repo-url>
```

Refresh the local cache of chart repos (run this before a search/install if
a repo's charts seem out of date):
```bash
helm repo update
```

List repos configured locally:
```bash
helm repo list
```

Search a repo (or all added repos) for a chart:
```bash
helm search repo <chart-name>
```

## Releases

List releases in a namespace:
```bash
helm list -n <namespace>
```

List releases across all namespaces:
```bash
helm list --all-namespaces
```

Install a chart as a new release:
```bash
helm install <release-name> <chart> -n <namespace>
```

Install/upgrade in one step (creates the release if it doesn't exist yet —
useful in CI where you don't want to branch on first-install vs upgrade):
```bash
helm upgrade --install <release-name> <chart> -n <namespace>
```

Upgrade an existing release with a values file:
```bash
helm upgrade <release-name> <chart> -n <namespace> -f values.yaml
```

Uninstall a release:
```bash
helm uninstall <release-name> -n <namespace>
```

## Inspecting Releases

Show the values currently in use for a release (merged defaults +
overrides):
```bash
helm get values <release-name> -n <namespace>
```

Show all the values (including chart defaults) for a release, not just the
overrides:
```bash
helm get values <release-name> -n <namespace> --all
```

Show the full rendered manifest that was actually applied for a release:
```bash
helm get manifest <release-name> -n <namespace>
```

Show release history (past revisions, useful before a rollback):
```bash
helm history <release-name> -n <namespace>
```

Roll back a release to a previous revision:
```bash
helm rollback <release-name> <revision-number> -n <namespace>
```

## Dry Runs & Debugging

Render a chart's templates locally without installing/upgrading anything —
good for sanity-checking output before it hits the cluster:
```bash
helm template <release-name> <chart> -n <namespace> -f values.yaml
```

Simulate an install/upgrade against the cluster (validates against the
Kubernetes API, unlike `helm template`) without actually applying it:
```bash
helm upgrade --install <release-name> <chart> -n <namespace> -f values.yaml --dry-run
```

Show what changed between the current release and a would-be upgrade
(needs the `helm-diff` plugin):
```bash
helm diff upgrade <release-name> <chart> -n <namespace> -f values.yaml
```

Increase verbosity on any command to see what Helm is doing under the hood
(e.g. chart resolution, API calls):
```bash
helm upgrade --install <release-name> <chart> -n <namespace> --debug
```

## Chart Development

Lint a local chart directory for common mistakes before packaging/installing:
```bash
helm lint ./<chart-dir>
```

Package a local chart directory into a versioned `.tgz`:
```bash
helm package ./<chart-dir>
```

Show the default values.yaml shipped with a chart:
```bash
helm show values <chart>
```
