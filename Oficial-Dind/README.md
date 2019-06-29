# Configuring DIND = Docker in Docker

I used this [oficial link](https://support.cloudbees.com/hc/en-us/articles/360019236771-How-to-build-my-own-docker-images-in-CloudBees-Core-on-Modern-Cloud-Platforms) the support CloudBees.

# In Master Jenkins

Click on Manage Jenkins and Kubernetes Pod Template

### Add Kubernetes Pod Template

```
Name   : pod-dind
Labels : pod-dind
Usage  : Only build jobs with label expressions matching the node
```

![Image08](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image08.png)
Image08

### Add DinD Container

```
Name                   : dind
Docker image           : docker:18.06.1-ce-dind
Working directory      : /home/jenkins
Allocate pseudo-TTY    : checked
Run in privileged mode : checked
```

![Image09](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image09.png)
Image09

![Image10](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image10.png)
Image10

### Test pipeline Job

```
podTemplate(){
    node('pod-dind') {
        container('dind') {

            stage('Build My Docker Image') { 
                sh 'docker info'

                sh 'touch Dockerfile'
                sh 'echo "FROM centos:7" > Dockerfile'
                
                sh "cat Dockerfile" 
    
                sh "docker -v" 
                sh "docker info" 
                sh "docker build -t my-centos:1 ." 
            } 
        }
    }
}
```
![Image11](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image11.png)
Image11
