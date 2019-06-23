# Docker DIND

I am using as Alpine base image odavid/jenkins-jnlp-slave, for use Docker in Docker.

[Github link](https://github.com/odavid/jenkins-jnlp-slave)

For this use make basics steps above:

My Dockerfile:

```
FROM odavid/jenkins-jnlp-slave

RUN echo "Criando minha imagem DIND"
```

In this Dockerfile can you customize for your environment, for example install curl:

```
FROM odavid/jenkins-jnlp-slave

RUN echo "Criando minha imagem DIND"
RUN apk add curl
```

Some adjusts

Click on Configure Jenkins / Management Jenkins and configure your this image: renizgo/jenkins-dind

See images above:

*Set image name*
![Image05](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image05.png)
Image05

*Set environment variable DIND=true*
![Image06](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image06.png)
Image06

*Set Privileged mode*
![Image07](https://raw.githubusercontent.com/renizgo/helm-jenkins-minikube/master/images/image07.png)
Image07
