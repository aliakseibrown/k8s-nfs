# Large Sacle Computing - Lab 6 – Kubernetes on Kind

## Prerequisites

Docker, kind, kubectl, Helm

## Quickstart

```bash
# 1. Install dependencies
brew install kind
brew install kubectl

# 2. Create kind cluster
kind create cluster --name lab6-cluster

# 3. Install NFS provisioner
helm repo add nfs-ganesha-server-and-external-provisioner https://kubernetes-sigs.github.io/nfs-ganesha-server-and-external-provisioner/
helm install my-release nfs-ganesha-server-and-external-provisioner/nfs-server-provisioner --set=storageClass.name=my-nfs-storage

# 3. Deploy app
kubectl apply -f pvc.yaml
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
kubectl apply -f job.yaml

# 4. Check the job completed
kubectl get jobs

# 5. Check the pod is running
kubectl get pods

# 6. Access your service
kubectl port-forward svc/nginx-service 8080:80

# 7. Test
Visit http://localhost:8080 in your browser - you should see "Hello from Kubernetes Lab!"

```

## Structure
- NFS Provisioner: Provides shared storage for pods
- PVC (PersistentVolumeClaim): Requests storage from the provisioner
- NGINX Deployment: Serves web content from the shared storage
- Job: Writes content to the shared storage
- Service: Exposes NGINX to the network