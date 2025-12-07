学习Jenkins的目的
- 使用Jenkins来做Java, python, node的代码
- 在学习Jenkins的过程中, 接触到了Maven, 激发了一些学习java的需求
- 同时Jenkins中也有构建docker 镜像, 部署应用到kubernetes的动作, 所以需要学习完k8s后回来整合使用
- 


# 概念
- Jenkins 中的Job分两种类型 一种叫freestyle, 一种是pipeline模式
	- Freestyle 模式, 多是依赖于shell脚本
	- pipeline模式具备了 Pipline as Code 风格, 目前判断不是所有公司都已经接受了pipeline的模式. 所以两种方式都需要学习
	- 每个任务都在一个workspace中, 在这个任务后, 之前build的结构, 特别是生成的文件会影响后面的build, 比如文件创建什么的
- 在CICD中, 基本的步骤分为
	1. 拉去仓库
	2. 本地测试 unittest
	3. 本地docker image构建
	4. 本地集成测试
	5. 部署到预发环境
	6. 部署到生产环境
- Jenkins Architecture 分为controller / agent
	- Controller 负责编排任务, 收取执行结果
	- agent 负责执行
- Jenkins 需要以来很多的插件来完成对于不同开发语言的构建
### 重要的目录
创建的job都会放在一下目录 `/var/jenkins_home/workspace/Hello-pipeline`
如果在build的过程中, 创建了文件, 也可以在着目录下看到

# GUI
- Stages View 可以看到一个job有多少stage
# 在学习环境中构建 Jenkins
### 使用docker来安装Jenkins
使用docker compose来安装
参考
https://github.com/vdespa/install-jenkins-docker?tab=readme-ov-file

Dockerfile
```yaml
FROM jenkins/jenkins:lts-jdk21
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
	https://download.docker.com/linux/debian/gpg
	RUN echo "deb [arch=$(dpkg --print-architecture) \
	signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
	https://download.docker.com/linux/debian \
	$(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
```
构建 my-jenkins image
```shell
docker builid -t my-jenkins .
```

`docker-compose.yaml`
```yaml
services:
  jenkins-docker:
    image: docker:dind
    container_name: jenkins-docker
    privileged: true
    environment:
      - DOCKER_TLS_CERTDIR=/certs
    volumes:
      - jenkins-docker-certs:/certs/client
      - jenkins-data:/var/jenkins_home
	ports:
	  - "2376:2376"
    networks:
      jenkins:
        aliases:
          - docker
    command: --storage-driver overlay2

  my-jenkins:
    image: my-jenkins
    build:
      context: .
	container_name: my-jenkins
    restart: on-failure
    environment:
	  - DOCKER_HOST=tcp://docker:2376
	  - DOCKER_CERT_PATH=/certs/client
	  - DOCKER_TLS_VERIFY=1
	volumes:
	  - jenkins-data:/var/jenkins_home
	  - jenkins-docker-certs:/certs/client:ro

	ports:
	  - "8080:8080"
	  - "50000:50000"
	networks:
	  - jenkins
  
networks:
  jenkins:
  driver: bridge

volumes:
  jenkins-docker-certs:
  jenkins-data:

```
通过docker compose 创建实例
```shell
docker compose up -d 
docker compose down # 关闭容器

# 删除全部volumes和image

docker compose down --volumes --rmi all
```


# Pipeline 
为了解决freestyle job almost impossible to track 的问题. 使用 Jenkins pipeline file 脚本来编辑
基本结构. 因为是code之后, 又可以和git合作, 利用github进行多人协同. 使得代码更透明, 并可重复
- stages
- post
	- success
	- always
- actions
	- cleanWs()
	- archiveArtifacts
```groovy
pipeline {
    agent any
    stages {
        stage('Prepare') {
            steps {
	            cleanWs() // 可以直接在开始的地方运行
                sh 'mkdir -p build'
                sh 'touch build/computer.txt'
                sh 'echo "Mainboard" >> build/computer.txt'
            }
        }
        stage('Build') {
            steps {
                sh '''
                touch build/computer.txt
                echo "Mainboard" >> build/computer.txt
                '''
            }
        }
    }

    post {
	    success {
		    archiveArtifacts artifacts: 'build/**'
	    }
        always {
            cleanWs()
        }
    }
}
```
