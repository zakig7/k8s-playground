# k8s-playground

A simple Kubernetes playground repository with example manifests for deploying and experimenting with workloads, autoscaling, services, and HTTP routing.

This repo contains a set of Kubernetes manifests that demonstrate deploying multiple microservices, exposing them via services and HTTPRoutes, and configuring autoscaling with Horizontal Pod Autoscalers.

## What’s Inside

This repository includes sample Kubernetes resources for exploring core Kubernetes patterns:

### Applications

| Component | Purpose |
|-----------|---------|
| **api** | Backend API service |  
| **web** | Frontend web service |  
| **crawler** | Auxiliary/worker service for background jobs |  

Each component includes:

- Deployment
- Service
- ConfigMap (where applicable)
- Optional HPA
- HTTP routing

### Examples

- `api/` — API backend Deployment  
- `web/` — Frontend Deployment  
- `crawler/` — Worker Deployment  
- `workloads/test*/` — Simple CPU & RAM test pod  
- `*/hpa.yaml` — Horizontal Pod Autoscaler examples  
- `*/service.yaml` — Corresponding Service objects  
- `web/httproute.yaml` — Sample HTTPRoute definitions (for Gateway API)  
- ConfigMaps for component configuration

## Getting Started

### Prerequisites

- Kubernetes cluster (Minikube, Kind, k3d, GKE, etc.)
- `kubectl` configured to communicate with your cluster
- Optional: Gateway API CRDs installed if using **HTTPRoute**

### Apply all manifests

To deploy all example workloads:

```sh
kubectl apply -k .
```

This will create deployments, services, autoscalers, configmaps, and routes.

### Verify Resources

Check that pods are running:

```sh
kubectl get pods
```

View services:

```sh
kubectl get svc
```

Check HPAs:

```sh
kubectl get hpa
```

### Expose or Access Services

If you’re running a local cluster (e.g. Minikube), you can port-forward a specific service:

```sh
kubectl port-forward svc/web-service 8080:80
```

## Workloads & Scaling

This repo includes basic HPA examples to learn how Kubernetes autoscaling works:

```sh
kubectl autoscale deployment web-deployment --cpu-percent=50 --min=1 --max=3
```

## Repository Structure

```sh
k8s-playground/
├── README.md
├── namespaces/
│   ├── crawler.yaml
│   └── playground.yaml
│
├── gateway/
│   ├── gatewayclass.yaml
│   └── gateway.yaml
│
├── apps/
│   ├── api/
│   │   ├── configmap.yaml
│   │   ├── deployment.yaml
│   │   │── httproute.yaml
│   │   │── pvc.yaml
│   │   └── service.yaml
│   │
│   ├── web/
│   │   ├── configmap.yaml
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   ├── hpa.yaml
│   │   └── httproute.yaml
│   │
│   └── crawler/
│       ├── configmap.yaml
│       ├── deployment.yaml
│       └── service.yaml
│
├── workloads/
│   ├── testcpu/
│   │   ├── deployment.yaml
│   │   └── hpa.yaml
│   │
│   └── testram/
│       ├── deployment.yaml
│       └── configmap.yaml
│
└── kustomization.yaml
```
