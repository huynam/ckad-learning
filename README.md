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

### Namespace

Tenant unit that ensure isolations with the rest.
Byt default, we use the default namespace. But when creating a multitenant cluster, you can segegrate and isolate service per ns

ns can define min resource spec guarantee but also quotas.

To access pod in different namespace
`<name svs>.<name ns>.svc.cluster.local`

--namespace dev

namespace in the metadata section

Switch to dev namespace:
`kubectl config set-config $kubectl config current-contrxt) --namespace=dev`
`kubectl get pods --all-namespaces`


### CMD and Arg in kubernetes

```
spec:
  containers:
    - name: <docker container name>
      image: <docker image>
      command:["sleep2.0] //override entrypoing

      args: ["10"]

```

### environment variable

```
spec:
  containers:
  - name: <bla>
    image: <bla>
    env:
      - name: <env name>
        value: <value>
      - name: <env name>
        valueFrom:
          configMapKeyRef: // from config map
      - name: <env name>
        valueFrom:
          secretKeyRef: // from secret

```

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

`kubectl run nginx --image nginx --port 80 --expose`

ENTRYPOINT ["sleep"] appended
CMD ["5"] override at dcoker run

run --entrypoint override
