# containerized-nexus2

Helm chart for Sonatype Nexus Repository Manager 2 (`sonatype/nexus:2.14.19-01`), deployed with Argo CD.

## Deploy

Requires Argo CD in the `argocd` namespace. Argo CD deploys what is on `main` in GitHub, so push changes first.

```bash
kubectl apply -f argocd/application.yaml
kubectl -n argocd wait app/nexus2 --for=jsonpath='{.status.sync.status}'=Synced --timeout=5m
kubectl -n argocd wait app/nexus2 --for=jsonpath='{.status.health.status}'=Healthy --timeout=10m
```

## Access

```bash
kubectl -n nexus2 port-forward svc/nexus2 8081:8081
```

Open http://localhost:8081/nexus and log in as `admin` / `admin123`, then change the password.

## Update

Edit `values.yaml`, commit, and push to `main`. Argo CD syncs automatically.

## Remove

```bash
kubectl delete -f argocd/application.yaml   # removes Nexus, keeps its data (PVC)
kubectl delete namespace nexus2             # also deletes the data
```
