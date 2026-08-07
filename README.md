# Kubernetes Deployments & ReplicaSets

This repository contains my hands-on practice with Kubernetes Deployments and ReplicaSets using Minikube.

## What I Learned

- What is a Deployment
- What is a ReplicaSet
- How a Deployment creates Pods
- How to create a Deployment using a YAML file
- How to view Deployments, ReplicaSets, and Pods
- How Kubernetes automatically creates a new Pod if one is deleted (Self-Healing)
- How to watch Pod status in real time

---

## Tools Used

- Kubernetes
- Minikube
- kubectl
- Docker
- Ubuntu Linux

---

## Deployment YAML

This project uses a simple Nginx Deployment with **3 replicas**.

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

---

## Commands I Practiced

Create the Deployment

```bash
kubectl apply -f deployment.yaml
```

View Deployments

```bash
kubectl get deployments
```

View ReplicaSets

```bash
kubectl get rs
```

View Pods

```bash
kubectl get pods
```

View Pod Details

```bash
kubectl get pods -o wide
```

Watch Pods Live

```bash
kubectl get pods -w
```

Delete a Pod

```bash
kubectl delete pod <pod-name>
```

Delete the Deployment

```bash
kubectl delete deployment nginx-deployment
```

---

## Self-Healing Demo

I deleted one of the running Pods.

```bash
kubectl delete pod nginx-deployment-xxxxxxxx
```

After deleting the Pod, Kubernetes automatically created a new Pod because the Deployment was configured with **3 replicas**.

The Pod lifecycle looked like this:

```
Running
   ↓
Terminating
   ↓
Pending
   ↓
ContainerCreating
   ↓
Running
```

This is called **Self-Healing** in Kubernetes.

---

## Kubernetes Architecture

```
Deployment
      │
      ▼
ReplicaSet
      │
 ┌────┼────┐
 ▼    ▼    ▼
Pod1 Pod2 Pod3
```

The Deployment manages the ReplicaSet.

The ReplicaSet makes sure the required number of Pods are always running.

---

## Repository Structure

```
.
├── deployment.yaml
└── README.md
```

---

## Conclusion

In this project, I learned how Kubernetes Deployments work, how ReplicaSets manage Pods, and how Kubernetes automatically replaces failed or deleted Pods to maintain the desired state.
