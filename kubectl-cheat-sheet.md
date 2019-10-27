# Kubectl Commands
* https://kubernetes.io/docs/reference/kubectl/cheatsheet
* very good tuto: https://medium.com/google-cloud/kubernetes-101-pods-nodes-containers-and-clusters-c1509e409e16

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
* to get all resources by namespace. ex for namespace default 
```
k get all -n default
```
* log of a pod (-f for tailing)
```
k logs -f my-pod
```
* log from a container within a POD
```
kubectl logs <pod-name> -c <init-container-2>
```
* delete a resource, ex a pod
```
k delete pod my-pod
```
or more deneric, all resources associated to a yaml
```
k delete -f mypod.yaml
```
* delete many resources associated to a label
```
k delete configmaps,StatefulSets,PodDisruptionBudget,services,pods -l label_key=label_value
```
* Port forwarding: After opening the app ports: ports section added to the deployment manifest, 
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: some-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sommeapp
  template:
    metadata:
      labels:
        app: sommeapp
    spec:
      containers:
      - name: some-container
        image: some-image
        ports:                                      
        - containerPort: 3000                      
          name: http                               
        - containerPort: 22                        
          name: ssh      
```
To see the ports
```
kubectl describe deployment | grep Ports
```
we can proceed to port forwarding from the host to a pod (debug only, the proper way is to use a service)
```
 kubectl port-forward $PODNAME 3000:3000
 ```
 kubectl will forward all connections on port 3000 on my local machine into the pod running in the cloud
 
* Attaching a container running ik k8s (similar to docker exec command)
```
kubectl exec -it podname --container containername -- /bin/bash
kubectl exec zk-0 -- /opt/zookeeper/bin/zkCli.sh create /hello world
``
