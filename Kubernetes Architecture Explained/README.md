# Kubernetes Architecture Overview

Kubernetes (K8s) is a powerful orchestration tool for managing containerized applications. This document provides an easy-to-understand breakdown of Kubernetes architecture and its components.

## Kubernetes Architecture

Kubernetes follows a master-worker architecture consisting of two main types of nodes:

- **Master Node**: Controls and manages the cluster.
- **Worker Nodes**: Run the actual workloads (containers).

### 1. Master Node Components

The master node is the brain of the Kubernetes cluster. It manages the overall state of the cluster and ensures that the desired state matches the actual state.

#### a. API Server

- **What it does**: Acts as the front-end for the Kubernetes control plane.
- **How it works**: All communication (e.g., from users, worker nodes, or external systems) goes through the API Server.
- **Example**: When you run `kubectl apply -f deployment.yaml`, the request goes to the API Server.

#### b. etcd

- **What it does**: A key-value store that stores all cluster data (e.g., configurations, states).
- **How it works**: Acts as the single source of truth for the cluster.
- **Example**: When you create a deployment, its configuration is stored in etcd.

#### c. Scheduler

- **What it does**: Assigns workloads (Pods) to worker nodes based on resource availability and constraints.
- **How it works**: Watches for newly created Pods and assigns them to the best-suited node.
- **Example**: If a Pod requires 2 CPUs and 4GB RAM, the Scheduler finds a node with available resources.

#### d. Controller Manager

- **What it does**: Ensures the desired state of the cluster matches the actual state.
- **How it works**: Runs controllers (e.g., Node Controller, Deployment Controller) that monitor and manage cluster resources.
- **Example**: If a Pod crashes, the Deployment Controller ensures a new Pod is created to replace it.

### 2. Worker Node Components

Worker nodes are where the actual workloads (containers) run. Each worker node has the following components:

#### a. Kubelet

- **What it does**: Acts as the agent on each worker node.
- **How it works**: Communicates with the API Server to ensure containers are running as expected.
- **Example**: If a Pod is scheduled on a node, the Kubelet ensures the container is started.

#### b. Kube Proxy

- **What it does**: Manages network communication between Pods and services.
- **How it works**: Implements load balancing and routing rules for network traffic.
- **Example**: If a service needs to communicate with a Pod, Kube Proxy ensures the traffic is routed correctly.

#### c. Container Runtime

- **What it does**: Runs the containers (e.g., Docker, containerd).
- **How it works**: Pulls the container image and runs the container as specified in the Pod definition.
- **Example**: Runs a Docker container for your application.

### 3. Key Kubernetes Objects

These are the building blocks of Kubernetes that define how applications are deployed and managed.

#### a. Pod

- **What it is**: The smallest deployable unit in Kubernetes.
- **How it works**: A Pod can contain one or more containers that share the same network and storage.
- **Example**: A Pod running a web server and a logging sidecar container.

#### b. Service

- **What it is**: Provides a stable IP address and DNS name for a group of Pods.
- **How it works**: Acts as a load balancer for Pods.
- **Example**: A Service exposing a web application running on multiple Pods.

#### c. Deployment

- **What it is**: Manages the lifecycle of Pods (e.g., scaling, updates).
- **How it works**: Ensures the desired number of Pods are running and handles rolling updates.
- **Example**: A Deployment running 3 replicas of a web application.

#### d. ConfigMap and Secret

- **What they are**: Store configuration data and sensitive information (e.g., passwords, API keys).
- **How they work**: Injected into Pods as environment variables or files.
- **Example**: A ConfigMap storing database connection details.

## How Components Work Together

Let's walk through an example of deploying an application in Kubernetes:

1. **User Creates a Deployment**:

   - You write a `deployment.yaml` file and run `kubectl apply -f deployment.yaml`.
   - The request goes to the API Server.

2. **API Server Stores the Deployment in etcd**:

   - The Deployment configuration is stored in etcd.

3. **Scheduler Assigns Pods to Nodes**:

   - The Scheduler assigns the Pods to worker nodes based on resource availability.

4. **Kubelet Runs the Pods**:

   - The Kubelet on each worker node pulls the container image and starts the Pod.

5. **Controller Manager Ensures Desired State**:

   - The Deployment Controller ensures the desired number of Pods are running.

6. **Kube Proxy Handles Networking**:

   - The Kube Proxy sets up networking rules so the Pods can communicate with each other and external services.

7. **Service Exposes the Application**:

   - A Service provides a stable endpoint for accessing the application.

## Visual Representation of Kubernetes Architecture

```
+-------------------+
|    Master Node    |
|-------------------|
| API Server        |
| etcd              |
| Scheduler         |
| Controller Manager|
+-------------------+
         ^
         |
         v
+-------------------+
|   Worker Node 1   |
|-------------------|
| Kubelet           |
| Kube Proxy        |
| Container Runtime |
| Pods              |
+-------------------+
         ^
         |
         v
+-------------------+
|   Worker Node 2   |
|-------------------|
| Kubelet           |
| Kube Proxy        |
| Container Runtime |
| Pods              |
+-------------------+
```

## Key Takeaways

- **Master Node**: Manages the cluster (API Server, etcd, Scheduler, Controller Manager).
- **Worker Nodes**: Run the workloads (Kubelet, Kube Proxy, Container Runtime).
- **Pods**: The smallest deployable units.
- **Services**: Provide stable networking for Pods.
- **Deployments**: Manage the lifecycle of Pods.

This document provides a simplified explanation of Kubernetes architecture. For more details, refer to the [official Kubernetes documentation](https://kubernetes.io/docs/).

