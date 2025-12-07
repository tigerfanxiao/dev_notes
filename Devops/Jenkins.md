# 在学习环境中构建 Jenkins
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

基本结构
```groovy
pipeline { // 标准开头
	agent any // 表示jenkins cluster中任意的agent
	stages("build") { // 阶段
		steps {
			echo 'building the application...'
		}
	}
	stages("test") {
		steps {
			echo 'testing the application'
		}
	}
	stages("deploy") {
		steps {
			echo 'deploying the application'
		}	
	}
}
```
