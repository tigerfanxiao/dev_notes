all docker file will have name as `Dockerfile`

container for node

```bash
FROM ubuntu:lastest
RUN apt update
RUN apt install python3 -y

WORKDIR /usr/app.src

COPY print.py ./

CMD ["python3", "./print.py"]
```

container for python

create a folder and put the requirement.txt in it

make a requirement.txt file

```
requests
```

找一个官方python版本的image和tag

RUN run the command one file when this container first created

CMD run the command when the container is started, which means when we stop and restart the container the command will be re-run

```
FROM python:3.9-slim-buster
COPY ./requirements.txt ./requirements.txt
COPY ./main.py ./main.py
RUN pip3 install -r requirements.txt
CMD ["python3", "./main.py"]
```

Build image from dockerfile

```bash
docker build -t <image-name> .  # docker deamon will pick the local Dockerfile 
```

run the container from image

```bash
docker run <image-name>
```

without -d will run the container and get the result, the container will stop when you close the terminal

CMD vs ENTRYPOINT

```bash
FROM ubuntu
MAINTAINER sofija
RUN apt-get update
ENTRYPOINT ["echo", "Hello"]
CMD ["World"]
```

ADD src .