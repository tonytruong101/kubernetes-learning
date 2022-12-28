## Overview

Here is an example of a Kubernetes manifest file for a single pod deployment:

```
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: example-container
    image: example-image
```

This manifest file creates a single pod with a single container running the "example-image" image. The pod will be named "example-pod".

To deploy this pod on a Kubernetes cluster, you can use the kubectl command-line tool:

```
kubectl apply -f example-pod.yaml
```
This will create the pod in the cluster and start the container.

You can also use the kubectl command to view the status of the pod and the logs of the container:

```
kubectl get pods
kubectl logs example-pod
```
Keep in mind that a single pod deployment is suitable only for simple, non-critical applications or for testing purposes. For production deployments, you will typically want to use a higher-level controller such as a Replica Set or a Deployment to ensure reliability and scalability.
