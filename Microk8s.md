# Microk8s

Microk8s is a minikube like single node k8s installation.  

## Install
Very easy  
https://ubuntu.com/kubernetes/install

## Get started
This page also list all the addons we can install like DNS storage, dashboard ...
https://microk8s.io/docs/

## The dashboard
https://microk8s.io/docs/addon-dashboard

## Using Helm
https://webcloudpower.com/use-kubernetics-locally-with-microk8s/

## Config (not sure yet it's the proper way)

I got the microk8s aka mk8s config with  
```
microk8s.kubectl config view --raw
```
Then I copied all the corresponding sections to /home/pjmd/.kube/config but Helm still see minikube.
