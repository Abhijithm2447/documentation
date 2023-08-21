# Docker Installation

## Linux

[doc](https://www.linuxbaya.com/2020/09/edgex-foundry-and-its-setup.html)

## Docker compose without daemon

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2 vim

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum update -y 

sudo yum install -y docker-ce docker-ce-cli containerd.io

mkdir -p /etc/systemd/system/docker.service.d

systemctl daemon-reload

systemctl enable docker

systemctl start docker

systemctl status docker

sudo docker run hello-world
```

## Docker compose

```bash
sudo yum update
sudo yum upgrade
sudo yum install curl
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
/usr/local/bin/docker-compose
```
