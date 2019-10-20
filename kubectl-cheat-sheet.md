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
* get the log of a pod
```
k logs -f my-pod
```
* delete a resource, ex a pod
```
k delete pod my-pod
```
or more deneric, all resources associated to a yaml
```
k delete -f mypod.yaml
```
* After opening the app ports listed in the deployment manifest, 
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
we can proceed to port forwarding from the host to a pod (debug only)
```
 kubectl port-forward $PODNAME 3000:3000
 ```
 kubectl will forward all connections on port 3000 on my local machine into the pod running in the cloud
 
