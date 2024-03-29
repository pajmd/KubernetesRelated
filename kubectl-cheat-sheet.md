# Kubernetes

## Kubectl Commands
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
```
* Running a image, ex: one which contains netools
```
k run netshell -it --rm --image tutum/dnsutils -- sh
```
to resume
```
kubectl attach netshell-c46ff599-bqxdm -c netshell -i -t' command when the pod is running
```
## Debugging
Check service endpoints
```
k get endpoints zookeeper-headless-service
```
If there are no endpoints there must be a mismatch beteen the selector and the labels.

See https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/#does-the-service-have-any-endpoints

TL;DR: check spec.selector field of your Service actually selects for metadata.labels values on your Pods

## Kubernetes features

### Volumes
see https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-volumes-example-nfs-persistent-volume.html

### Multi container pods
https://www.mirantis.com/blog/multi-container-pods-and-container-communication-in-kubernetes/



## GCloud Platform
https://cloud.google.com/kubernetes-engine/docs/quickstart?refresh=1

### Switching context minikube / GCP
```
kubectl config use-context CONTEXT_NAME
```
to list all contexts:
```
kubectl config get-contexts
```
### Gainning acces to a NodePort service running on GCP
https://cloud.google.com/kubernetes-engine/docs/how-to/exposing-apps

* find the external IPs of the nodes:
```
k get node --output wide
```
* create a firewall rule to allow access
```
gcloud compute firewall-rules create test-node-port --allow tcp:30284
Nodeport bein 30284
```

### Stop VM Instances
https://cloud.google.com/compute/docs/instances/stop-start-instance
TDL;DR
From the main menu,go to Compute Engine, in Instance Groups, remove the VMs from the Group, tehn go to VM instances and stop the VMs.

### HELM

Tiller needs to be install on GKE.  
To avoid the annoying issues I experimented when installing it on my machine minikube I installed it on GKE
following the instructions I described in my HELM page (from my GOProject workspace ...)  
See https://github.com/pajmd/KubernetesRelated/blob/master/helm.md

#### Issue with helm commands

running install or list ... return errors
```
helm install some_chart
Error: no available release name found
```
```
helm list
Error: configmaps is forbidden: User "system:serviceaccount:kube-system:default" cannot list resource "configmaps" in API group "" in the namespace "kube-system"
```

Found a workaround at:
https://stackoverflow.com/questions/43499971/helm-error-no-available-release-name-found

TL;DR:
```
kubectl create clusterrolebinding permissive-binding --clusterrole=cluster-admin --user=admin --user=kubelet --group=system:serviceaccounts
```

