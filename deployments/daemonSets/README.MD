## Overview
A DaemonSet is a controller in Kubernetes that ensures that a copy of a pod is running on every node in the cluster. It is used to deploy system-level services that need to run on every node, such as logging agents or monitoring agents.

Here is an example of a Kubernetes manifest file for a DaemonSet:

```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: example-daemonset
spec:
  selector:
    matchLabels:
      app: example
  template:
    metadata:
      labels:
        app: example
    spec:
      containers:
      - name: example-container
        image: example-image
```

This manifest file creates a DaemonSet named "example-daemonset" that manages a Replica Set with a copy of the pod running the "example-image" image on each node in the cluster. The DaemonSet will select pods with the label "app: example" to manage.

To deploy this DaemonSet on a Kubernetes cluster, you can use the kubectl command-line tool:

```
kubectl apply -f example-daemonset.yaml
```

You can use the kubectl command to view the status of the DaemonSet and the Replica Set and pods it manages:

```
kubectl get daemonsets
kubectl get replicasets
kubectl get pods
```

## When To Use

A DaemonSet is a good choice for deploying system-level services that need to run on every node in the cluster. It ensures that a copy of the pod is running on every node, even if new nodes are added to the cluster.
