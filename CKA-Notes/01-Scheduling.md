# 5% - Scheduling

## • Use label selectors to schedule Pods.
## • Understand the role of DaemonSets.
```
```
## • Understand how resource limits can affect Pod scheduling.

https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
## • Understand how to run multiple schedulers and how to configure Pods to use them.
```
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    component: scheduler
    tier: control-plane
  name: my-scheduler
  namespace: kube-system
spec:
  selector:
    matchLabels:
      component: scheduler
      tier: control-plane
  replicas: 1
  template:
    metadata:
      labels:
        component: scheduler
        tier: control-plane
        version: second
    spec:
      serviceAccountName: my-scheduler
      containers:
      - command:
        - /usr/local/bin/kube-scheduler
        - --address=0.0.0.0
        - --leader-elect=false
        - --scheduler-name=my-scheduler
        - --lock-object-name=my-scheduler
        image: gcr.io/my-gcp-project/my-kube-scheduler:1.0
```
```
master $ kubectl get pods -n kube-system
NAME                             READY   STATUS    RESTARTS   AGE
kube-scheduler-master            1/1     Running   0          6m49s
my-scheduler                     1/1     Running   0          6m49s
```
## • Manually schedule a pod without a scheduler.
```
apiVersion: v1
kind: Pod
metadata:
  name: annotation-second-scheduler
  labels:
    name: multischeduler-example
spec:
  schedulerName: my-scheduler
  containers:
  - name: pod-with-second-annotation-container
    image: k8s.gcr.io/pause:2.0
```
## • Display scheduler events.
```
kubectl get events -o wide
```
