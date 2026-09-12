# Kubernetes — Cluster Access

## Cluster Access

Log in via AWS SSO to get temporary credentials (open `<sso-start-url>` in a
browser, select the target account, then click "Access Keys" to reveal the
short-lived credentials to export below):
```cmd
set AWS_ACCESS_KEY_ID=<access-key-id>
set AWS_SECRET_ACCESS_KEY=<secret-access-key>
set AWS_SESSION_TOKEN=<session-token>
```

Update kubeconfig for a non-prod EKS cluster:
```bash
aws --region <region> eks update-kubeconfig --name <cluster-name>
```

Update kubeconfig for a pre-prod EKS cluster via an assumed IAM role
(`<role-arn>` is the ARN of the admin role to assume):
```bash
aws eks update-kubeconfig --region <region> --name <cluster-name> --role-arn <role-arn>
```

Get the API server endpoint for a cluster (`cluster.endpoint` is the HTTPS
URL kubectl/clients talk to, e.g. `https://ABCD1234EFGH5678.gr7.ap-south-1.eks.amazonaws.com`):
```bash
aws eks describe-cluster --name <cluster-name> --region <region> --query "cluster.endpoint" --output text
```

Every time you run `update-kubeconfig`, kubectl adds/updates a "context"
(cluster + user + namespace combo) locally. If you've connected to more than
one cluster, use these to see and switch between them instead of re-running
`update-kubeconfig`:

List all contexts you have configured locally:
```bash
kubectl config get-contexts
```

Show which context (cluster) you're currently pointed at:
```bash
kubectl config current-context
```

Switch to a different cluster/context:
```bash
kubectl config use-context <context-name>
```

## Namespaces

List all namespaces:
```bash
kubectl get ns
```
