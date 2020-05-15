# 19% - Core Concepts
## • Understand the Kubernetes API primitives.
### Imperative Commands with Kubectl
Reference: https://kubernetes.io/docs/reference/kubectl/conventions/

Test: https://kodekloud.com/courses/certified-kubernetes-administrator-with-practice-tests-labs/lectures/12038872

1. Create a pod
```
kubectl run nginx --image=nginx --restart=Never --dry-run -o yaml
```

2. Create a deployment
```
kubectl create deployment --image=nginx nginx --dry-run -o yaml
```

3. Create a service - clusterip
```
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```

4. Create a service - nodepod
```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```

5. Create 
## • Understand the Kubernetes cluster architecture.
### Master (Control Plane)
Roles: Manaage, Plan, Schedule, Monitor nodes

Components:
#### Kube-API server

#### ETCD
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/setup-ha-etcd-with-kubeadm/

#### Scheduler

#### Kube Controller Manager


### Worker Nodes.
Role: Host application as containers
Components:
#### Kubelet
#### Kube-proxy


### Kube-apiserver
### 
## • Understand Services and other network primitives.