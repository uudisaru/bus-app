```shellscript
# Inspired by https://cloud.google.com/kubernetes-engine/docs/tutorials/guestbook
# https://cloud.google.com/sdk/docs/quickstart-macos
gcloud init
gcloud components install kubectl
gcloud config set project bus-app-285213
gcloud container clusters create bus-app --num-nodes=4
gcloud container clusters list
gcloud container clusters describe bus-app

kubectl get nodes

# Kubernetes Deployment that runs a single replica Redis leader Pod
kubectl apply -f redis-leader-deployment.yaml
kubectl get pods
kubectl get deployments
kubectl logs deployment/redis-leader

# Redis leader service required to proxy the traffic to the Redis leader Pod
kubectl apply -f redis-leader-service.yaml
kubectl get service

# Redis replicas for high availability
kubectl apply -f redis-follower-deployment.yaml
kubectl get pods
kubectl apply -f redis-follower-service.yaml


# Poller
kubectl apply -f bus-poller-deployment.yaml


# Server
kubectl apply -f bus-server-deployment.yaml
kubectl apply -f bus-server-service.yaml

# Scaling
kubectl scale deployment bus-server --replicas=5
kubectl scale deployment bus-server --replicas=2


# Clean-up
kubectl delete service bus-server

# Wait until load balancer terminated
gcloud compute forwarding-rules list
gcloud container clusters delete bus-app
```