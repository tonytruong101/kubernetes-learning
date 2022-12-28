## Overview
A Kubernetes Persistent Volume (PV) is a piece of storage in the cluster that has been provisioned by an administrator. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual pod that uses the PV. This means that the data in the PV continues to exist after the pod is deleted.

Here is an example of how to create a PV in Kubernetes:

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
  labels:
    type: local
spec:
  storageClassName: standard
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"

```

This example creates a PV named "example-pv" with a storage capacity of 10Gi and a hostPath of "/mnt/data". The PV can be accessed in read-write mode by a single pod. The storageClassName "standard" is used to bind the PV to a StorageClass, which specifies the underlying volume plugin and any associated options.

To use this PV in a pod, you can create a PersistentVolumeClaim (PVC) that requests storage from the PV. The PVC acts as an abstraction layer between the pod and the PV, allowing the pod to consume storage without needing to know the specifics of the underlying PV.

Here is an example of a PVC that claims storage from the PV created above:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

This PVC can then be referenced in a pod's specification to mount the PV's storage into the pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
  - name: example-container
    image: example-image
    volumeMounts:
    - name: example-pv-storage
      mountPath: "/mnt/data"
  volumes:
  - name: example-pv-storage
    persistentVolumeClaim:
      claimName: example-pvc

```

In this example, the container in the pod mounts the PV's storage at the path "/mnt/data". The PV's data will be available to the container as long as the PV and PVC exist in the cluster.

