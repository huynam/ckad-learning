## Notes from CKAD course from Udemy


### Preconfig

vim
```
set expandtab
set tabstop=2
set shiftwidth=2
```

:retab

bashrc:

alias k=kubectl



### Pods

POD is the smallest unit
POD, containers share the same network, namespace, volume

`kubectl run nginx —image nginx`
`kubectl create -f pod-definition.yaml`

Yaml format
```
apiVersion:
kind:
metadata:

spec:
```

extract yaml from existing pod

`kubectl get pod <podname> -o yaml > pod-definition.yaml`

`kubectl edit pod <podname>`

### Replication controllers/Replica Set

Replication controller helps running multiple instance of a pod or restart pod. Replica set is the new way of doing in k8s

Replica set can also manage pods that are not created as part of the replica set via selector. (New feature compared to the replication controller)

```
apiVersion: v1
kind: ReplicationController

spec:
  template:
    <Pod definition>
  replicas:<number of replicas>
```
```
apiVersion: v1
kind: ReplicaSet

spec:
  template:
    <Pod definition>
  replicas:<number of replicas>
  selector:
    matchLabels:
      <criteria>
```
Replicaset is the process to monitor the pods. It will ensure that we have a number of pods describe in the replicas attribute.

When creating the replicaset, if there are pods that match the selector, then it will only create the diff

To update the number of instance, edit the rcas replicas and:
`kubectl replace -f rcs-definition.yml`

alt:
`kubectl scale --replicas=6 -f <rcs file name>`
`kubectl scale --replicas=6 replicaset <rcs name>`

### Deployments

deployment automatically create a replicaset

template of deployment is identical of rcs.
kind: Deployment

`kubectl get all`


### Tips

--dry-run=client test if the command is valid

-o yaml to output into yaml

`kubectl run nginx --image nginx --dry-run=client -o yaml > pod.yml`
`kubectl create deployment --image nginx --dry-run=client -o yaml > deployment.yml`

Create a service:

`kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml > service.yaml`
if you select -type=Nodeport, the node port cannot be specified. Need to edit service.yml

or

`kubectl create service clusterip redis --tcp:6379:6379 --dry-run=client -o yaml > service.yml`
Limitation: need to add the selectors manually

-o json
-o wide All the info
-o yaml

