### Create
`
gcloud services enable compute.googleapis.com
`

### Verify any instances were created before
```
gcloud compute instances list
```

### Let's start by creating the VPC network and subnet.

In your network planning above you decided to use the IP address range 10.0.0.0/16 for the host network of your cluster.

Since the subnet is the network where your VM instances, and thus your Kubernetes nodes, will reside, you have to use this IP address range for your subnet.

```
gcloud compute networks create my-k8s-vpc --subnet-mode custom
gcloud compute networks subnets create my-k8s-subnet --network my-k8s-vpc --range 10.0.0.0/16
```

```
gcloud compute instances create my-k8s-master \
    --machine-type n1-standard-2 \
    --subnet my-k8s-subnet \
    --image-family ubuntu-1804-lts \
    --image-project ubuntu-os-cloud \
    --can-ip-forward
```