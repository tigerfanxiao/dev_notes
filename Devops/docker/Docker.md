[[Dockerfile]]

# Container

Containers are isolated execution environment that let us quickly and efficiently deploy exact copies of our desired environments. This is done by virtualizing at the operating system-level and isolating any libraries and applications within the container.

**Bare Metal**: No virtualization - the application or service is deployed directly onto the machine

### **Container VS virtual machine**

### **Container Runtime**

-   docker (Most Popular)
    
-   rkt
    
    Created by CoreOS, “designed with composability and security in mind.”
    
-   containerd
    
    Emphasizes “simplicity, robustness, and portability"
    

### **Container Use Case**

-   Solving Dev/Prod Parity
-   Migrating Infrastructure
-   Microservice-Based Architectures
-   Elastic Architectures
-   Self-Healing Architectures
-   CI/CD pipeline

# Docker

## **Concepts**

-   Docker hub: online official docker image repository
-   Docker is primarily a container runtime.

## **Uninstall old docker**

### **centos**

```
sudo yum remove -y docker \\
                  docker-client \\
                  docker-client-latest \\
                  docker-common \\
                  docker-latest \\
                  docker-latest-logrotate \\
                  docker-logrotate \\
                  docker-engine
```

## **Install docker**

### **Centos**

```
sudo yum install -y yum-utils \\
  device-mapper-persistent-data \\
  lvm2
```

## **Install Docker CE**

Add the Utilities needed for Docker:

```
sudo yum install -y yum-utils \\
  device-mapper-persistent-data \\
  lvm2
```

Set up the `stable` repository:

```
sudo yum-config-manager \\
    --add-repo \\
    <https://download.docker.com/linux/centos/docker-ce.repo>
```

Install Docker CE:

[](https://docs.docker.com/engine/install/centos/)[https://docs.docker.com/engine/install/centos/](https://docs.docker.com/engine/install/centos/)

```bash
sudo yum-config-manager --add-repo <https://download.docker.com/linux/centos/docker-ce.repo>
sudo yum install -y docker
# start docker and check the status
sudo systemctl start docker && sudo systemctl enable docker
```

Add `cloud_user` to the `docker` group:

```bash
sudo usermod -aG docker cloud_user # to avoid issue docker run with sudo
```

### **Ubuntu**

Remove existing Docker installs. update the ubuntu

```
sudo su
sudo apt update
```

And then install any prerequisite packages.

```
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

Note that many of these packages are already installed on our Playground server.

We can now add Docker's GPG key.

```
curl -fsSL <https://download.docker.com/linux/ubuntu/gpg> | sudo apt-key add -
```

And verify its configuration.

```
sudo apt-key fingerprint 0EBFCD88
```

To add the Docker repository, we now just need to run:

```
sudo add-apt-repository  "deb [arch=amd64] <https://download.docker.com/linux/ubuntu> $(lsb_release -cs) stable"
```

And update our server again.

```
sudo apt update
```

We can now install the necessary packages.

```
sudo apt install docker-ce docker-ce-cli containerd.io
```

To finish up, we want to add our `cloud_user` user to the `docker` group. Then you can run docker without `sudo`

```
sudo usermod cloud_user -aG docker
```

Log out then log back in to refresh the Bash session before running any `docker` commands.

## **Run Docker**

```
docker container ls -a
```

Now let's take a look at the output, starting at the steps provided after the `Hello from Docker!` message.

```
1. The Docker client contacted the Docker daemon.
2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
3. The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.
4. The Docker daemon streamed that output to the Docker client, which sent it to your terminal.
```

To start (`1`), the Docker client takes our command and relays it to the Docker daemon, which then (`2`) looks for the `hello-world` image. An **image** is the base server configuration that our containers are based on. This defines the operating system, any packages, and more. For example, the `hello-world` image is specifically set up to send us the provided message, while the `ubuntu` image is just the stock version of the latest version of Ubuntu.

```
docker run <image-name>
```

```
# list all the current running container
docker container ls
docker container ls -a  # list even stopped container
docker ps -a # same as "docker container ls -a"
docker ps -aq # only show the hash code
```

To launch a container and actually be able to work with it,

-   we can use the `i` or `-interactive`
-   `t` or `-tty` flags. `t` also make sure that the container will have persistent ip address

```
docker run -it --name <container-alias> <image name>
exit # exit the container
```

Note that we also assigned the container a name with `--name`.

-   `-restart allways` restart when the host server restart
-   `d` or `-detach`, which runs the container in the background

```
docker run -dt --name bg-container --restart always alpine
# Publish to host port
docker run -p <host_port>:<container_port> --dt --name <container> <image-name>
```

-   `-rm`Remove the container upon exit

```
docker run -it --name <container_name> --rm <image>
```

### **restarting parameters**

-   `no` - Never restart
-   `always` - Always restart when stopped
-   `unless-stopped` - Restart unless manually stopped
-   `on-failure<:retries>` - Restart when the container fails; we can supply the maximum amount of retries

```bash
# run command in container
docker exec <conainter_name> <command>
# interate with running container with specific shell
docker exec -it <container> <shell> # shell=/bin/bash
docker exec -it <container> <command>
# copy local file to container
docker cp <local_file> <container_name>:<container_file_path>
# copy file in container to local directory
docker cp <container_name>:<container_file_path> <local_directory>
# Stop and restart a container
docker start <container_name>
docker restart <container_name>
docker stop <container_name>
# remove container
docker rm <container_name>
# stop all the container
docker kill $(docker ps -aq)
# remove all stopped container
docker container prune
docker rm $(docker ps -aq)
# rename container
docker rename <old_name> <new_name>
# view all containers status
docker stats
# vieww specific container status
docker stats <container_name>
```

# Configure Container

```bash
# view docker file json
docker inspect <container_name>
# view container ip
docker inspect <container_name> | grep -i ip
```

# Container Management

```bash
docker ps # same as docker container ls
docker ps -a 

docker container ls # show running container
docker container ls -all # show running container including stopped ones

docker stop <container> # stop the runing container
docker start <container> 

docker container start <container> # start one stopped container
docker rm <container> # delete stopped container
docker container rm <container> # remove one container

docker container prune # remove all stopped container
docker container kill <container> # kill running container
```

Run container

```bash
docker run -d -p 80:80 docker/getting-started # -d detached way in background
docker run -p <host_port>:<container_port> --dt --name <container> <image-name>
```

# Image Management

```bash
docker image --help # 
docker image ls # list all images download
docker images # show local images

# create container run image
docker run <image-name>

# remove one local image
docker image rm <image_name>
docker rmi <image_name> # remove specific image
# remove all the images that no related to container
docker image prune -a
```

export images

```bash
docker save -o <path_for_tar_file> <image_name> # export image as file
docker load -i <path_for_tar_file> # load image file 

# push image to docker hub
docker login --username=<username>
docker tag <image_name> <username>/<repo-name>:latest # tag image
docker push <username>/<image_name> 
```

将一个正在运行的 container 保存为一个 image

```bash
docker images --help

# create image base on the container
docker commit <container_name> <image_name>
# 
docker save -o <path_for_tar_file> <image_name> # export image as file

docker load -i <path_for_tar_file> # load image file 
```

# Build Image

创建一个镜像, 会从本地插在 dockerfile

write a docker file

```bash
FROM node:10-alpine
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
WORKDIR /home/node/app  # change directory, 后面的命令都会以这个目录为基础运行
COPY package*.json ./ # Copies app package* files to the working directory
RUN npm config set registry <http://registry.npmjs.org/>
RUN npm install
# copy the all the local files to the docker working directory and set the owner to node:node
COPY --chown=node:node . .
USER node
EXPOSE 8080
CMD [ "node", "index.js" ] # another way to run command
```

build image by docker file

```powershell
docker build . -t <image_name> # if the docker file in local dir
docker build <docker_file> -t <username>:<image_name>
```

# Docker Volume

# Orchestration

Kubernetes is Orchestration tool to manage container

### **AWS EKS**

Amazon Elastic Kubernetes Service (EKS) is a managed service that makes it easy for you to run Kubernetes on AWS without needing to install and operate your own Kubernetes control plane or worker nodes.

### **Other Orchestration**

-   Marathon
    
    Based on Apache Mesos, offers APIs for integrating with other tools.
    
-   Docker Swarm
    
    Docker Swarm Docker’s native container orchestration solution
    
-   Nomad
    
    Open source and built by HashiCorp, designed to be simple and lightweight.
    

# Scenario

Zero-Downtime Deployments

A zero-downtime deployment (with containers) goes like this:

1.  Spin up containers running the new code.
2.  When they are fully up, direct user traffic to the new containers.
3.  Remove the old containers running the old code. No downtime for users!

Containers are great at accomplishing things like:

-   Software Portability – Running software consistently on different machines.
-   Isolation – Keeping individual pieces of software separate from one another.
-   Scaling – Increasing or decreasing resources allocated to software as needed.
-   Automation – Automating processes to save time and money.
-   Efficient Resource Usage – Containers use resources efficiently, which saves money