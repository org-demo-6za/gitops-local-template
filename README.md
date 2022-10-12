# gitops for local development of a cluster

[https://k3d.io/v5.3.0](https://k3d.io/v5.3.0)

```bash
k3d cluster create kubefirst --agents 3 --agents-memory 1024m  --registry-use k3d-registry:64606
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




# registry
k3d registry create                                           
INFO[0000] Creating node 'k3d-registry'                 
INFO[0001] Pulling image 'docker.io/library/registry:2' 
INFO[0002] Successfully created registry 'k3d-registry' 
INFO[0002] Starting Node 'k3d-registry'                 
INFO[0002] Successfully created registry 'k3d-registry' 
# You can now use the registry like this (example):
# 1. create a new cluster that uses this registry
k3d cluster create --registry-use k3d-registry:64606

# 2. tag an existing local image to be pushed to the registry
docker tag lite:1 localhost:64606/lite:1

# 3. push that image to the registry
docker push localhost:64606/lite:1

# 4. run a pod that uses this image
kubectl run mynginx --image localhost:64606/lite:1


apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-test-registry
  labels:
    app: nginx-test-registry
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-test-registry
  template:
    metadata:
      labels:
        app: nginx-test-registry
    spec:
      containers:
      - name: nginx-test-registry
        image: k3d-registry.localhost:64606/lite:1
        ports:
        - containerPort: 80

apiVersion: v1
kind: Pod
metadata:
  name: hello-world
  labels:
    app: hello
spec:
  containers:
    - name: hello-world-container
      image: 
      command: ['sh', '-c', 'echo Hello World! && sleep 3600']


https://github.com/ContainerSolutions/trow/blob/main/docs/USER_GUIDE.md


```

# big side step - argo admin kubectl boom
MESSAGE
Error (exit code 1): pods "testing-123-k8gmp-git-checkout-2935214226" is forbidden: User "system:serviceaccount:argo:default" cannot patch resource "pods" in API group "" in the namespace "argo"