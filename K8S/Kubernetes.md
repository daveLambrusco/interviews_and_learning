# Kubernetes

- [Kubernetes](#kubernetes)
  - [Core Concepts](#core-concepts)
    - [1. Cluster](#1-cluster)
    - [2. Worker Nodes](#2-worker-nodes)
    - [3. Pods](#3-pods)
  - [Master node](#master-node)

Kubernetes (often abbreviated as **K8s**) is an open-source platform designed to automate the deployment, scaling, and operation of application containers. Originally developed by Google, it is now maintained by the Cloud Native Computing Foundation (CNCF).

Kubernetes helps manage containerized applications across a cluster of machines, providing tools for:

- Deploying applications
- Scaling them as needed
- Managing changes to existing containerized apps
- Optimizing hardware usage

## Core Concepts

### 1. Cluster

A **Kubernetes cluster** consists of a set of machines (nodes) that run containerized applications. It has two main types of nodes:

- **Master Node (Control Plane)**: Manages the cluster.
- **Worker Nodes**: Run the actual applications.

### 2. Worker Nodes

A **worker node** is a machine (physical or virtual) that runs the applications in containers. Each worker node contains:

- **Kubelet**: An agent that communicates with the control plane.
- **Container Runtime**: Software like Docker or containerd to run containers.
- **Kube-proxy**: Handles networking and load balancing.

Worker nodes are where the actual workloads (containers) are executed.

### 3. Pods

A **pod** is the smallest and simplest unit in the Kubernetes object model. It represents a single instance of a running process in your cluster.

Key characteristics:

- A pod can contain one or more containers.
- Containers in a pod share the same network namespace and storage.
- Pods are ephemeral — they can be created, destroyed, and recreated by Kubernetes as needed.

## Master node

The **master node** (also called the **control plane**) is the brain of a Kubernetes cluster. It manages the cluster and makes global decisions about scheduling, scaling, and responding to events.

Key components:

1. API Server (`kube-apiserver`)

   - The **front-end** of the Kubernetes control plane.
   - All communication (from users, CLI, or other components) goes through the API server.
   - It validates and processes REST requests.

2. Scheduler (`kube-scheduler`)

   - Assigns newly created pods to nodes based on resource availability, policies, and constraints.
   - Makes decisions like: *"Which node should run this pod?"*

3. Controller Manager (`kube-controller-manager`)

   - Runs various controllers that handle routine tasks:
     - **Node Controller**: Monitors node health.
     - **Replication Controller**: Ensures the desired number of pod replicas.
     - **Endpoint Controller**: Manages endpoint objects.
     - **Service Account** and **Token Controllers**: Manage access and credentials.

4. etcd

   - A **distributed key-value store** used to store all cluster data.
   - It’s the **source of truth** for the cluster state.
   - Highly available and consistent.

5. Cloud Controller Manager (optional)

   - Integrates Kubernetes with cloud provider APIs (e.g., AWS, Azure, GCP).
   - Manages cloud-specific resources like load balancers, volumes, and networking.
