# Kubernetes Cluster Test

This repo contains a Kubernetes cluster and configuration procedure.

The main goal is to achieve a configuration based on the specified tasks below.

Relevant sources are listed at the bottom. 

## Tasks

- Set up a Kubernetes cluster using any preferred platform (e.g., Kind, Minikube, etc.).
- Configure Traefik as the ingress controller to manage access to services within the cluster.
- Deploy an Apache server to serve requests for mobile users.
- Deploy an NGINX server to serve requests for other users.
- Implement Horizontal Pod Autoscaler (HPA) for the NGINX deployment to automatically scale based on CPU utilization. The target CPU utilization should be set to 70%.
- Configure Cert-Manager to manage SSL certificates and ensure that both Apache and NGINX services are accessible via HTTPS.

## Procedure

### 1. Cluster creation

In this case I used Kind to run local Kubernetes, creating a cluster in Docker:
```
kind create cluster --name devops-test
```

### 2. Apache configuration

Deploy Apache:
```
kubectl create -f ./apache/apache-deployment.yml
```

Expose deployment in order to access Apache:
```
kubectl expose deployment/apache
```

### 3. NGINX configuration

Deploy NGINX:
```
kubectl create -f ./nginx/nginx-deployment.yml
```

Expose deployment in order to access NGINX:
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

> On Windows, Helm might fail saying `Access is Denied`. This is because it runs on the AppData folder and therefore needs admin privileges when executing the command.

### 5. Traefik configuration

Enable Kubernetes provider and define entrypoints:
```
kubectl apply -f ./traefik/traefik-config-map.yml
```

Set up Ingress:
```
kubectl apply -f ./traefik/traefik-ingress.yml
```

Set up redirect for Android requests:
```
kubectl apply -f ./traefik/traefik-android-redirect.yml
```

### 6. HPA for NGINX

This targets the NGINX deployment CPU usage. Note that `nginx-deployment.yml` has the CPU resource request set.
Parameters are set on the `nginx-hpa.yml` config file, apply directly:
```
kubectl apply -f ./nginx/nginx-hpa.yml
```

### 7. Cert-manager installation

Cert-manager needs an installation for its own resources:
```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.2/cert-manager.yaml
```

### 8. Cert-manager configuration

A cluster-wide issuer provides certificates when interacting with the previously configured Ingress.
Note that e-mail and issuer account have been made up for this example.
Apply directly:
```
kubectl apply -f ./cert-manager/cert-manager-cluster-issuer.yml
```

> This command may throw out an error about cert-manager's webhook. This is because the Kubernetes API needs to trust the webhook's certificate. Waiting a few seconds and retrying the command should be enough for a correct creation.

## Sources

- [Kind Installation and User Guide](https://kind.sigs.k8s.io/)
- [Service Deployment](https://kubernetes.io/docs/tutorials/services/connect-applications-service/)
- [Apache Deployment Example](https://www.devopstricks.in/deploy-apache-kubernetes/)
- [Kubernetes Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Kubernetes Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)
- [Traefik Installation](https://doc.traefik.io/traefik/getting-started/install-traefik/)
- [Traefik Ingress](https://doc.traefik.io/traefik/providers/kubernetes-ingress/)
- [Horizontal Pod Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)
- [Cert-manager Installation](https://cert-manager.io/docs/installation/kubectl/)
- [Cert-manager: Securing Ingress Resources](https://cert-manager.io/docs/usage/ingress/)
- [DevOps GitLab by NB Tech Support](https://gitlab.com/nb-tech-support/devops)