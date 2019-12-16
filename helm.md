# helm

__NOTE:__  
Helm install procedure described below is about version 2.n.  
The newer version 3 as of 201911 may well be easier to install because tiller running on the cluster is no longer. 


## Docs
https://helm.sh/docs/developing_charts/

## ApiVersion
https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-apiversion-definition-guide.html

## Installing helm client and tiller server

After installing helm with snap I got the following error when trying to initialize it:  
```
> helm init --canary-image --upgrade
$HELM_HOME has been configured at /home/pjmd/.helm.
Error: error installing: the server could not find the requested resource
```
The diverse solution proposed in google didn't work until I got to 
https://github.com/helm/helm/issues/3996: dazdaz 

Here are the steps I followed:
* rebuilt helm. see: https://helm.sh/docs/using_helm 
* exectuded the following commands:  
```
> kubectl delete service tiller-deploy -n kube-system
>  kubectl delete pods tiller-deploy -n kube-system --force=true --timeout=0s
>  kubectl delete deployment tiller-deploy -n kube-system --force=true --timeout=0s
> kubectl -n kube-system delete deploy tiller-deploy
> cd ~/GoProjects/src/k8s.io/helm/bin
>  ./helm init --service-account default --tiller-image gcr.io/kubernetes-helm/tiller:v2.15.0-rc.1
>  kubectl -n kube-system get pods
```
## Intermittent issues:  
```
pec.containers{tiller} readiness probe failed: Get http://172.17.0.6:44135/readiness: dial tcp 172.17.0.6:44135: connect: connection refused
```
To see the log:  
```
kubectl logs --namespace kube-system tiller-deploy-557875fd46-dtsdh
```
```
helm version
```
Should show both the clinet and server (tiller) running and therefore  
```
helm list
```
Should return 

## useful commands

* To add the Incubator repository/charts to a local client, run helm repo add:
```
$ helm repo add incubator https://kubernetes-charts-incubator.storage.googleapis.com/
"incubator" has been added to your repositories
```
* To search the incubator repo:
```
helm search incubator
```
* To update dependencies defined in requirements.yaml
```
cd folder_where_requiremens.yaml_is
helm dep up .
```

* install a release
```
helm install <chartfolder/>
```
* To uninstall i.e. delete a release
``
helm delete <release name>
```
* list what was released
```
helm list [--deployed]
```
* list deleted releases
```
helm list --deleted
```


