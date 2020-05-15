# 5% - Scheduling

## • Use label selectors to schedule Pods.
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
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
## • Understand the role of DaemonSets.
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: elasticsearch
  namespace: kube-system
  labels:
    k8s-app: elasticsearch
spec:
  selector:
    matchLabels:
      name: elasticsearch
  template:
    metadata:
      labels:
        name: elasticsearch
    spec:
      containers:
      - name: elasticsearch
        image: k8s.gcr.io/fluentd-elasticsearch:1.20
```
## • Understand how resource limits can affect Pod scheduling.
Test: https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests-labs/lectures/12038835
### Default CPU and memory request 
https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/
```
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
```
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```


## • Understand how to run multiple schedulers and how to configure Pods to use them.
https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
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
