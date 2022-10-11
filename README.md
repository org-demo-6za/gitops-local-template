# gitops for local development of a cluster

[https://k3d.io/v5.3.0](https://k3d.io/v5.3.0)

```bash
k3d cluster create kubefirst --agents 3 --agents-memory 1024m 
```

## helm install argocd with values yaml 
```bash
helm repo add argo https://argoproj.github.io/argo-helm
"argo" has been added to your repositories

helm install argocd --namespace argocd --create-namespace -f ./components/localhost/argocd-values.yaml --version 4.10.2 argo/argo-cd

export AWS_ACCESS_KEY_ID=k-ray
export AWS_SECRET_ACCESS_KEY=feedkraystars

kubectl -n minio port-forward svc/minio 9000:9000

aws --endpoint-url http://localhost:9000 s3 ls
```