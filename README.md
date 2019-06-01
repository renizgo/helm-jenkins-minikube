# Install Helm Jenkins on Minikube example

## Description:

In this tutorial you will install Jenkins Master and Jenkins Agent using Docker in Docker.
This install is will be in Kubernetes Environment using Minikube System

## Requirements

* Git clone in this repository
* Uninstall previous Minikube version
* Install teh last version Minikube 
* Kubectl command installed
* 

### Git clone repository

```
git clone https://github.com/renizgo/helm-jenkins-minikube.git
cd helm-jenkins-minikube
```

### Uninstall previous version for Minikube

```
chmod +x minikube_uninstall.sh
./minikube_uninstall.sh
```
![Image01](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image01.png)
Image01

### Install the last minikube version

In MacOS user is very simple installation using brew:

```
brew cask install minikube
```

You can also install it on MacOS by download a static binary:

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 \
  && chmod +x minikube
```

Copy Minikube executable to path:

```
sudo mv minikube /usr/local/bin
```

For another versions Operational System verify this [Oficial Link.](https://kubernetes.io/docs/tasks/tools/install-minikube/)

For verify installation:

```
minikube version
minikube version: v1.1.0
```

Start a minikube version:

```
minikube start --kubernetes-version=v1.14.0
```


