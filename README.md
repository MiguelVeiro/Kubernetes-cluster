# DevOps Technical Test

This repo explains the steps and commands followed to build the cluster and its configuration.
All sources are listed at the bottom. 

## Tasks
- Set up a Kubernetes cluster using any preferred platform (e.g., Kind, Minikube, etc.).
- Configure Traefik as the ingress controller to manage access to services within the cluster.
- Deploy an Apache server to serve requests for mobile users.
- Deploy an Nginx server to serve requests for other users.
- Implement Horizontal Pod Autoscaler (HPA) for the Nginx deployment to automatically scale based on CPU utilization. The target CPU utilization should be set to 70%.
- Configure Cert-Manager to manage SSL certificates and ensure that both Apache and Nginx services are accessible via HTTPS.

## Procedure

### 1. Cluster creation

In this case I used Kind to run local Kubernetes, creating a cluster in Docker:
```
kind create cluster
```

### 2. Apache configuration

Deploy Apache:
```
kubectl apply -f apache/apache-deployment.yml
```

Expose deployment in order to access Apache:
```
kubectl expose deployment/apache
```

### 3. Nginx configuration

Deploy Nginx:
```
kubectl apply -f nginx/nginx-deployment.yml
```

Expose deployment in order to access Nginx:
```
kubectl expose deployment/nginx
```

### 4. Traefik installation

Installing Traefik into the cluster via `helm`:
```
helm repo add traefik https://traefik.github.io/charts
helm repo update
helm install traefik traefik/traefik
```

### 5. Traefik configuration

Enable Kubernetes provider and define entrypoints:
```
kubectl apply -f traefik/traefik-config-map.yml
```

Enable Kubernetes Ingress:
```
kubectl apply -f traefik/traefik-ingress.yml
```

Set up redirect for Android requests:
```
kubectl apply -f traefik/traefik-android-redirect.yml
```


## Sources

- [Kind Installation and User Guide](https://kind.sigs.k8s.io/)
- [Service Deployment](https://kubernetes.io/docs/tutorials/services/connect-applications-service/)
- [Traefik Installation](https://doc.traefik.io/traefik/getting-started/install-traefik/)
- [Traefik Ingress](https://doc.traefik.io/traefik/providers/kubernetes-ingress/)
- []()