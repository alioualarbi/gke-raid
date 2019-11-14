# gke-raid
## Walkthrough

1. #### Creating a GKE cluster

```
gcloud container clusters create [CLUSTER_NAME] \
    --num-nodes 2 --local-ssd-count [NUMBER_OF_DISKS]

```
where [NUMBER_OF_DISKS] is the desired number of disks as an absolute number.

To create a node pool with local SSD disks in an existing cluster:

```
gcloud beta container node-pools create [POOL_NAME] --cluster [CLUSTER_NAME] \
    --num-nodes 1 --local-ssd-count [NUMBER_OF_DISKS]

```
The ```--local-ssd-count ``` specifies the number of local SSDs to be created per node. The maximum number varies by the machine type and region (see Local SSDs). When nodes are created, the local SSDs are automatically formatted and mounted on the host OS at the subdirectory /mnt/disks/, with each local SSD mounted at a "ssd#" directory.

2. #### Install RAID DaemonSet

The demonset will be in charge of creating the Raid0 disk on startup in this example we used 2 local SSD but you can modify ``` --local-ssd-count``` 

```
kubectl apply -f deamonset.yml
```
3. #### Deploying test pod

In this exmple we doploy just a pod with the ssd-array created by the deamonset and mount it.

```
kubectl apply -f test-pod.yml
```

4. #### Ssh to test-pod to verify

ssh to the pod from your cloud-shell or any instance the has the cluster credentials

```
kubectl exec -it test-pod -- /bin/bash
```
