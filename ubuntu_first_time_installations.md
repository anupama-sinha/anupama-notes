## Docker Installation
* Install Docker
> sudo apt install docker.io
* Check version of Docker
> docker –version
* Check status of Docker
> sudo systemctl status docker
* Enable Docker
> sudo systemctl enable --now docker

> Created symlink /etc/systemd/system/multi-user.target.wants/docker.service → /lib/systemd/system/docker.service.
* Test Docker command
> sudo docker run hello-world
* Removing Docker Engine
> sudo apt-get remove docker docker-engine docker.io containerd runc
* Below gets get.docker.com scripts in ubuntu home & downloads repo from download.docker.com
curl -fsSL https://get.docker.com -o get-docker.sh
> sudo sh get-docker.sh
* Using as non-root user
> sudo usermod -aG docker anupamasinha
* Check Container Status
> sudo docker ps -a
* Fixing Docker Admin Access
> sudo groupadd docker

> sudo usermod -aG docker ${USER}

> su -s ${USER}
* Command to remove all containers :
> docker container rm -f $(docker container ls -aq) 

And, then start Docker journey!!!

## Minikube Installation
>  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

>  sudo install minikube-linux-amd64 /usr/local/bin/minikube

> minikube start
😄  minikube v1.16.0 on Ubuntu 20.04
✨  Automatically selected the docker driver
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.20.0 preload ...
    > preloaded-images-k8s-v8-v1....: 491.00 MiB / 491.00 MiB  100.00% 2.01 MiB
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.20.0 on Docker 20.10.0 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
💡  kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default

> minikube dashboard

> minikube status

> curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"

> chmod +x ./kubectl

> kubectl version --client

## Jenkins Installation
> sudo service jenkins restart

> sudo service jenkins stop

> sudo service jenkins start
* Jenkins by default uses port 8080 as it uses Tomcat
* Also master slave communication happens in port 50000. So that port also needs to be exposed to bind it to master slave communication
-d : detached mode/background
-v : volume. To persist data on jenkins
* Running Jenkins from Docker image and running Github repository code:
> docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts 

## MYSQL Installation
> sudo apt install mysql-server

> systemctl status mysql.service
* Check user,password 
> sudo cat /etc/mysql/debian.cnf
* Start MySQL Terminal
> mysql -u debian-sys-maint -p

> SELECT User, Host, plugin FROM mysql.user;

> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root';



## References
* https://kubernetes.io/docs/tasks/tools/install-kubectl/
* https://www.jenkins.io/doc/book/pipeline/shared-libraries/
* https://www.jenkins.io/doc/book/installing/linux/#debianubuntu
* https://www.youtube.com/watch?v=M7_mZXh8h8A
* https://stackoverflow.com/questions/42421585/default-password-of-mysql-in-ubuntu-server-16-04
* https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04#step-4-%E2%80%94-testing-mysql
