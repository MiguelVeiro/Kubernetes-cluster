# DevOps Technical Test
This repo explains the steps and commands followed to build the cluster and its configuration.
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
`kind create cluster`

### 2. Traefik installation



## Tools
A number of tools that were used in this technical test:
- Docker Desktop
- kubectl
- VSCode Kubernetes extension

## Sources
