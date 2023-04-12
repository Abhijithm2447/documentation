---
description: Django app docker creation
---

# Dockerizing Django

#### Create django project

```bash
$ django-admin startproject DockerTestDjango
$ cd DockerTestDjango/
```

### Create Docker file with no extension

#### Dockerfile

```dockerfile
FROM python:3.8-slim-buster
ENV PYTHONUNBUFFERED 1
RUN mkdir /app
WORKDIR /app
COPY requirements.txt /app/
RUN pip install -r requirements.txt
COPY . /app/
EXPOSE 5002
```

### Create docker-compose.yml

```yaml
version: '3'

services: 
    web:
        image: abhijith/opencv
        build: .
        command: python3 manage.py runserver 0.0.0.0:5002
        volumes: 
            - .:/app
        ports: 
            - "5002:5002"            
```

### Build Docker image

```bash
$ sudo docker-compose up -d 
```

### Remove images

<pre class="language-bash"><code class="lang-bash">sudo docker container prune
docker rmi &#x3C;your-image-id>
# delete all unused images
<strong>sudo docker image prune
</strong></code></pre>

### stop docker

```bash
sudo systemctl stop docker
# restart
sudo systemctl start docker
```

### opencv libgl error

add the following line in Docker file

```docker
RUN apt-get update && apt-get install ffmpeg libsm6 libxext6  -y
```

complete code

```dockerfile
FROM python:3.8-slim-buster
ENV PYTHONUNBUFFERED 1
RUN mkdir /app
WORKDIR /app
RUN apt-get update && apt-get install ffmpeg libsm6 libxext6  -y
COPY requirements.txt /app/
RUN pip3 install -r requirements.txt
COPY . /app/
EXPOSE 5002
```

## Share docker image

[reference](https://bobcares.com/blog/move-docker-container-to-another-host/)

### export container

```bash
docker export container-name | gzip > container-name.gz
```

