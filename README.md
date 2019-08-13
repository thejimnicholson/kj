# Jenkins on Docker/Kubernetes

## Useful Links

### Windows-specific

+ [Run docker commands in WSL using Docker for Windows](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly) (Note that the section on changing the mount point did not work for me, and left me with a broken WSL that had to be removed/reinstalled.)
+ [Use kubectl from wsl with Docker for Windows Kubernetes](https://devkimchi.com/2018/06/05/running-kubernetes-on-wsl/)

### Kubernetes Dashboard

Useful for all manner of things, including getting access to container logs.

+ Install the dashboard following the instructions in the readme for the [project](https://github.com/kubernetes/dashboard)

Bearer token authentication worked for me on a Mac, failed mysteriously on Windows. 

+ [Create a user/set up the bearer token](https://github.com/kubernetes/dashboard/wiki/Creating-sample-user)

For Windows, [this devblog post](https://devblogs.microsoft.com/premier-developer/bypassing-authentication-for-the-local-kubernetes-cluster-dashboard/) will let you bypass the token challenge

When you want to access the dashboard, you will need to run `kubectl proxy` to let yourself in, or provision a service that lets you reach it. 

## Jenkins Master

+ Build the docker image with `docker build -t thejimnicholson/my-jenkins-image:1.0`

+ Deploy it `kubectl create -f jenkins-deployment.yaml`
+ Create a service to expose it `kubectl create -f jenkins-service.yaml`


### Accessing Jenkins UI

I couldn't get the UI to show on Windows, because I couldn't figure out what the IP of the Kubernetes cluster is. I ended up doing a port-forward to get into the UI:

```
kubectl port-forward jenkins-68ddf4f48b-5nkvv 8080:8080
```

## Jenkins agent configuration

+ Follow instructions for "jenkins slave setup" in [this article](https://www.blazemeter.com/blog/how-to-setup-scalable-jenkins-on-top-of-a-kubernetes-cluster/), ignore the parts about minikube. The deployment and service files included here are taken from that article. 

## gogs

## mysql 

https://kubernetes.io/docs/tasks/run-application/run-single-instance-stateful-application/#deploy-mysql

kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -ppassword

