# Kubectl Commands

alias k kubectl

* apply a yaml manifest to a cluster
```
kubectl apply -f stuff.yaml
```
* list the pods on the cluster
```
kubectl get pods
```
* get the pod or any service yaml
```
kubectl get pods (services) ... -o yaml
```
* get the description
```
k describe service my-service
```
* get the log of the created thing
```
k logs -f my-pod
```
