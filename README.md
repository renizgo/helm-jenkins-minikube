# Install Helm Jenkins on Minikube example

## Description:

In this tutorial you will install Jenkins Master and Jenkins Agent using Docker in Docker.
This install is will be in Kubernetes Environment using Minikube System

## Requirements

* Git clone in this repository
* Uninstall previous Minikube version
* Install teh last version Minikube 
* Kubectl command installed
* Helm installed

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

![Image02](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image02.png)
Image02

### Install kubectl command

For installation in others operational systems see [Oficial Link](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

In MacOS, download the latest version:

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl
```

Make the kubectl binary executable:

```
chmod +x ./kubectl
```

Move binary in to your PATH:

```
sudo mv ./kubectl /usr/local/bin/kubectl
```

Verify installed version:

```
kubectl version
``` 

### Helm installation

For installation in others operational systems see [Oficial Link](https://helm.sh/docs/using_helm/#installing-helm)

1. Download you [desired version](https://github.com/helm/helm/releases);

2. In this example:
```
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.14.0-darwin-amd64.tar.gz
```

3. Unpack it
```
tar -xvzf helm-v2.14.0-darwin-amd64.tar.gz
```

4. Move to you PATH:
```
mv darwin-amd64/helm /usr/local/bin/
```

5. Verify installed version
```
helm version
```

### Installer Tiller on server environment

1. Create a file tiller-rbac.yml:
```

apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system

---

apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

2. Execute this file:
```
kubectl create -f tiller-rbac.yml --record --save-config
```

3. Initialize Helm tiller
```
helm init --service-account tiller
```

4. Rollout verify:
```
kubectl -n kube-system rollout status deploy tiller-deploy
```

5. Verify if tiller pod is running
```
kubectl -n kube-system get pods
```

![Image03](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image03.png)
Image03

6. Update repository and verify Jenkins chart
```
helm repo update
helm search jenkins
```

```
NAME          	CHART VERSION	APP VERSION	DESCRIPTION
stable/jenkins	1.1.23       	lts        	Open source continuous integration server. It supports mu...
```

# Install Jenkins via Helm

With one simple command to install Jenkins:
```
helm install stable/jenkins --name jenkins --namespace jenkins --values jenkins-values.yml
```

Rollout checkout:
```
kubectl -n jenkins rollout status deploy jenkins
deployment "jenkins" successfully rolled out
```

Attention with this information:
```
NOTES:
1. Get your 'admin' user password by running:
  printf $(kubectl get secret --namespace jenkins jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
2. Get the Jenkins URL to visit by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace jenkins -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=jenkins" -o jsonpath="{.items[0].metadata.name}")
  echo http://127.0.0.1:8080
  kubectl --namespace jenkins port-forward $POD_NAME 8080:8080

3. Login with the password from step 1 and the username: admin
```

Access your environment:

![Image04](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image04.png)
Image04

Create a simple Pipeline for example

```
node {
    stage('Hello') {
        sh '''
        echo 'hello world'
        docker ps
        '''
    }    
}
```

Configurar no agente
always pull image
Environment DIND=true
Privileged=true
Install plugin Pipeline

