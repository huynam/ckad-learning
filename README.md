### Notes from CKAD course from Udemy

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