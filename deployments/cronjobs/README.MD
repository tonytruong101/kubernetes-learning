## Overview

A CronJob is a special type of Job in Kubernetes that is triggered by a schedule rather than by an external event. It is used to run periodic jobs on a schedule.

Here is an example of a Kubernetes manifest file for a CronJob:

```
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: example-cronjob
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: example-container
            image: example-image
          restartPolicy: OnFailure
  successfulJobsHistoryLimit: 3
  failedJobsHistoryLimit: 1
```
This manifest file creates a CronJob named "example-cronjob" that runs a Job every minute (according to the schedule specified in the schedule field). The Job runs a single instance of the "example-image" container and is configured to restart on failure. The CronJob is also configured to retain the last three successful jobs and the last failed job.

To deploy this CronJob on a Kubernetes cluster, you can use the kubectl command-line tool:

```
kubectl apply -f example-cronjob.yaml
```

You can use the kubectl command to view the status of the CronJob and the jobs it creates:

```
kubectl get cronjobs
kubectl get jobs
```

## When to use

A CronJob is a good choice for running periodic jobs on a schedule. It ensures that the job is run at the specified intervals, even if the pod or container fails.

