# Minikube Kubectl

## Creating pod from a private registry with a self signed certificate

After setting up my private registry with a self signed certificate (see DockerWorkspace/Docker.md), creating a pod would issue an error:  
```
$ >kubectl -f nhs_app_k8s.yaml 
pod/nhsappk8s created

$ >k describe pod nhsappk8s
....
Events:
  Type     Reason     Age              From               Message
  ----     ------     ----             ----               -------
  Normal   Scheduled  <unknown>        default-scheduler  Successfully assigned default/nhsappk8s to minikube
  Normal   Pulling    4s               kubelet, minikube  Pulling image "pjmd-ubuntu.com/nhs-server-app:v0.0.4"
  Warning  Failed     4s               kubelet, minikube  Failed to pull image "pjmd-ubuntu.com/nhs-server-app:v0.0.4": rpc error: code = Unknown desc = Error response from daemon: Get https://pjmd-ubuntu.com/v2/: x509: certificate signed by unknown authority
  Warning  Failed     4s               kubelet, minikube  Error: ErrImagePull
  Normal   BackOff    2s (x2 over 3s)  kubelet, minikube  Back-off pulling image "pjmd-ubuntu.com/nhs-server-app:v0.0.4"
  Warning  Failed     2s (x2 over 3s)  kubelet, minikube  Error: ImagePullBackOff
```

To fix the issue (there may be a better I haven't found) I copied the cert I created for the private repo to minikue:  
```
$ >minikube ssh
# >sudo mkdir -p /etc/docker/certs.d/pjmd-ubuntu.com
# >cd /etc/docker/certs.d/pjmd-ubuntu.com
# >sudo scp pjmd@pjmd-ubuntu.com:/etc/docker/certs.d/pjmd-ubuntu.com/domain.crt .
```

The obvious problem with this solution is when I delete the minikube VM I will have to copy the cert again.  
