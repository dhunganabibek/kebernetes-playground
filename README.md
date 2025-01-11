# Kubernetes Templates

## Kubernetes Components

Api Server

- front end for Kubernetes control plane.
- all commands goes to api server.
- do authentication, authorization and admission.
- exposes Kubernetes api.
- handle restful request and update the state in etcd

etcd

- distributed key value pair backing store for all cluster data.
- ensure data consistency by using Raft Consensus algorithm

Scheduler

- find best bit node for your pods.

## What happen when you do kubectl run nginx --image nginx

API Server:

- The API server receives the request and validates it.
- The API server writes the deployment object to etcd, the key-value store that holds the cluster's state.
etcd:

etcd

- stores the deployment configuration and ensures it is consistent and highly available.
- acts as the single source of truth for the cluster state.

Controller Manager

- The deployment controller in the controller manager notices the new deployment object in etcd.
- The deployment controller creates a ReplicaSet object to manage the desired number of pod replicas.
- The ReplicaSet controller then creates the necessary Pod objects.

Scheduler:

- The scheduler watches for new pods that do not have an assigned node.
- The scheduler selects an appropriate node for each pod based on resource availability and other constraints.
- The scheduler updates the Pod objects with the selected node information.

Kubelet:

- The kubelet on the selected worker node notices the new pod assignment.
- The kubelet pulls the nginx image from the container registry if it is not already available locally.
- The kubelet starts the container using the container runtime (e.g., Docker, containerd).

Kube-proxy:

- Kube-proxy on each node updates the network rules to allow communication to the new pods.
- It ensures that traffic can be routed to the pods based on the service configuration.

Pod Lifecycle:

- The pod is now running on the worker node, and the nginx container is serving requests.
- The kubelet continuously monitors the pod to ensure it is running as expected.

## Syntax of declarative of Kubernetes Files

```yaml
apiVersion: v1
kind: Pod
metadata: 
    name: nginx
    labels: 
        run: nginx
spec:
    containers: 
        - image : nginx
          name: nginx
```

## Namespaces in Kubernetes

kubectl get ns
NAME              STATUS   AGE   
default           Active   6m6s - The default namespace for user-created resources without a specified namespace.  
kube-node-lease   Active   6m6s - Contains lease objects for node heartbeat data to improve node controller performance.
kube-public       Active   6m6s - A publicly readable namespace for cluster information accessible to all users.
kube-system       Active   6m6s  - The namespace for Kubernetes system components and critical cluster resources.

creating a namespace

```bash
kubectl create ns testing
```

delelting a  namespace

```bash
kubectl delete ns testing
```