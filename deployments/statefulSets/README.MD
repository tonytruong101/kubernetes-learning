## Overview
A StatefulSet is a controller in Kubernetes that manages the deployment of stateful applications, where each instance of the application has a unique identity and persistent storage. It is used to deploy applications that require a unique network identity and stable storage, such as databases.

Here is an example of a Kubernetes manifest file for a StatefulSet:

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: example-statefulset
spec:
  serviceName: example-service
  replicas: 3
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
  volumeClaimTemplates:
  - metadata:
      name: example-pvc
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi

```
This manifest file creates a StatefulSet named "example-statefulset" that manages a Replica Set with three replicas of a pod running the "example-image" image. The StatefulSet is associated with a service named "example-service" that provides a stable network identity for the pods.

The StatefulSet also includes a volumeClaimTemplate that specifies a PersistentVolumeClaim (PVC) for each pod. The PVC will request 1Gi of storage with read-write-once access mode. The StatefulSet will create a unique PVC for each pod, ensuring that each pod has a unique, persistent volume.

To deploy this StatefulSet on a Kubernetes cluster, you can use the kubectl command-line tool:

```
kubectl apply -f example-statefulset.yaml
```

You can use the kubectl command to view the status of the StatefulSet and the Replica Set and pods it manages:

```
kubectl get statefulsets
kubectl get replicasets
kubectl get pods

```

## When to Use

A StatefulSet is a good choice for applications that require a unique network identity and stable storage, such as databases. It ensures that each instance of the application has a unique identity and persistent storage, even if the pod is deleted and recreated.
