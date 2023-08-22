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

#### **Docker Engine and Docker Compose Installation/Setup**

1. Installing pre-requisite and docker tools:

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2 vim
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum update -y
sudo yum install -y docker-ce docker-ce-cli containerd.io
sudo mkdir /etc/docker
cd /etc/docker
vi daemon.json
```

Paste the below content inside daemon.json:

```
{
"exec-opts": ["native.cgroupdriver=systemd"],
"log-driver": "json-file",
"log-opts": {
"max-size": "100m"
},
"storage-driver": "overlay2",
"storage-opts": [
"overlay2.override_kernel_check=true"
]
}
```

Save the daemon.json file:

```
:wq!
```

```
mkdir -p /etc/systemd/system/docker.service.d
systemctl daemon-reload
systemctl enable docker
systemctl start docker
```

Verify the installation:

```
systemctl status docker
sudo docker run hello-world
sudo docker image ls
```

2.  Install/setup Docker Compose:

    Update Repositories and Packages -

    ```
    sudo yum update
    sudo yum upgrade
    ```

    Install curl utilities If not installed -

    ```
    sudo yum install curl
    ```

    Download Docker Compose -

    ```
    sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```

    Change the file permissions to make the software executable -

    ```
    sudo chmod +x /usr/local/bin/docker-compose
    ```

    Check by running the below command whether Docker Compose is working -

    ```
    docker-compose
    ```

#### **Activity 2 - Searching, Pulling, and Displaying local images**

***

1. Log in to the Docker Hub account, explore different docker images by typing image name in the search explorer. Try image like: java, python, nginx, alpine, accenture/adop-nginx etc.
2. To list the different flags available with search use the below command -

```
docker search --help
```

3. Use the below command to search for an image using the docker cli –

```
# docker search <term>
docker search adop
```

4. To list all sub-commands available with the **`docker image`** management command use -

```
docker image --help
```

5. To pull an image from a registry (default is `hub.docker.com`) use –

```
# New Way
# docker image pull <IMAGE-NAME>[:<TAG>]
docker image pull alpine

# Old Way
# docker pull <IMAGE-NAME>[:<TAG>]
docker pull busybox:1.28.1
```

**NOTE**: If tag is not specified, default is always `latest`.

6. To display list of all local images use -

```
# New Way
docker image ls

# Old Way
docker images
```

#### **Activity 03 - Executing an image and listing containers**

***

1. To list all sub-commands available with the **`docker container`** management command use –

```
docker container --help
```

2. To view all options available to work with **`run`** use the below command -

```
docker container run --help
```

3. To execute a simple docker image use the below command –

```
# New Way
docker container run hello-world
# Old Way
docker run hello-world
```

**NOTE**: The hello-world image is not pulled again when the command is executed second time because it is cached locally.

4. To create a container from an existing image (i.e. alpine) and execute a command inside it use -

```
docker container run alpine echo "Hello, World"
```

**NOTE**: After the command execution, the container exits irrespective of the outcome.

5. To list the directory of alpine image by running `ls` command inside a container use -

```
docker container run alpine ls
```

6. To list the processes running inside container of alpine image use `ps` command with arguments `-ef` -

```
docker container run alpine ps -ef
```

**NOTE**: Ideally, there should only be a single process running inside a container.

7. To list all available options with the **`container ls`** sub-command use –

```
docker container ls --help
```

8. To list all running containers use –

```
# New Way
docker container ls

# Old Way
docker ps 
```

9. To list all running and stopped containers use the option `-a` or `--all` with `ls` –

```
# New Way
docker container ls -a

# Old Way
docker ps -a
```

**NOTE**: When you run a docker container without using the `--name` option, the docker engine assigns a name to that container. The name is comprised of an adjective and a famous personality last-name, joint by an underscore (\_).

10. Try out the below commands with different options.

```
# display all the containers including exited one
docker container ls -a 
# display only the containers id including exited one
docker container ls -aq
# display the last container started 
docker container ls -l 
# display only the id of last container started
docker container ls -lq 
```

11. The `--filter` option can be used to filter out the containers based on a key=value –

```
# docker container ls --filter key=value
docker container ls --filter status=exited
```

[REFERENCE LINK - docker container ls](https://docs.docker.com/engine/reference/commandline/container\_ls/)

[SUPPORTED FILTERS](https://docs.docker.com/engine/reference/commandline/ps/#filtering)

#### **Activity 04 - Using a container with terminal**

***

1. To use a container with terminal requires two different options –

i. `-i`, `--interactive` - It keeps STDIN open even if not attached.

ii. `-t`, `--tty` - Allocates a pseudo-TTY

2. To open an interactive pseudo terminal use -

```
# New Way
docker container run --interactive --tty alpine sh
docker container run -i -t alpine sh
docker container run -it alpine sh

# Old Way
docker run --interactive --tty alpine sh
docker run -i -t alpine sh
docker run -it alpine sh
```

* Add a new file by using **`touch`** command
* Then list down the directory and files inside the container using **`ls`** command
* Type **`exit`** to shutdown and exit the terminal.

**NOTE**: When the terminal exits, then the container is shut down.

```
docker container ls -al
```

3. The new container that uses the same image will run with its thin writeable layer. Therefore, the file saved earlier is not present. Try it out!

```
docker container run -it alpine sh

# on the container shell type ls to list directories and files 
ls
```

**NOTE**: To keep the container running rather than shut it down, use the keys combination of (**`ctrl + p + q`**).

4. To exec into container shell once again use -

```
# New Way
docker container exec -it <CONTAINER-ID|NAME> sh
# Old Way
docker exec -it <CONTAINER-ID|NAME> sh
```

**NOTE**: Typing the command `exit` does not terminates the container if its shell is accessed using `exec`; therefore, there is no need to use the keys combination of **`ctrl + p + q`**.

***

#### **Activity 05 - Running container in detached mode**

***

1. The information or logs of the container gets displayed directly on the host client, when the container ran without using the **`--detach`** or **`-d`** option.

```
docker container run alpine ping localhost -c 4
```

2. The **`--detach`** or **`-d`** option does not log the container information, and it runs the container in detached mode.

```
docker container run --detach alpine ping localhost -c 100
# OR
docker container run -d alpine ping localhost -c 100
```

3. To see the list of running containers use -

```
docker container ls
```

4. To attach the client back to the docker container use the attach command –

```
# New Way
docker container attach <CONTAINER-ID|NAME>

# Old Way
docker attach <CONTAINER-ID|NAME>
```

[Reference link - docker container attach](https://docs.docker.com/engine/reference/commandline/attach/)

#### **Activity 06 - Running a web server application container**

***

1. To pull the nginx web server image use -

```
docker image pull nginx:alpine
```

2. The `--name` option assigns a name to the container, instead of default name given by the docker engine.
3. The `-P` (capital P) option publishes the container port, which are exposed, to a random port on the host.
4. To run the nginx webserver on host machine published on random port in detached mode use -

```
docker container run --detach --name webserver -P nginx:alpine
```

5. To view the published port use the `container ls` command -

```
docker container ls
```

6. Open a new web browser tab window, and type the URL as given below.

```
http://<HOST-IP-ADDRESS>:<PUBLISHED-PORT>
```

**NOTE**: `HOST-IP-ADDRESS` is ther server IP Address received in Email ID.

7. The `--publish` or `-p` option binds the container ports that are exposed to specific ports on the host machine.

```
# <HOST-PORT>:<CONTAINER-PORT>
docker container run --detach --name webserver2 --publish 80:80 nginx:alpine
# OR
docker container run -d --name webserver2 -p 80:80 nginx:alpine
```

**NOTE**: The above command binds port 80 of the container to TCP port 80 of the host machine. The default protocol is TCP unless specified otherwise.

8. We can run more nginx containers with different names and its ports published on different host ports that are vacant.

```
docker container run -d --name webserver3 -p 8888:80 nginx:alpine
```

9. To list down the running containers use -

```
docker container ls
```

10. Open multiple new web browser tab window, and access the different web server instance running on respective host ports.

***

#### **Activity 07 - Debugging a container application**

***

1. To view all available options of the **`container logs`** management command use –

```
docker container logs --help
```

2. To fetch the container logs use –

```
# New Way
# docker container logs [OPTIONS] <CONTAINER>
docker container logs webserver

# Old Way
# docker logs [OPTIONS] <CONTAINER>
docker logs webserver
```

3. The **`-f`** or **`--follow`** option with logs command helps to follow log output.

```
docker container logs --follow webserver
```

**NOTE**: If we refresh the home page of the **`webserver`** instance, it outputs new log entries.

4. The **`--tail`** option displays the container logs information from the end depending on number passed as an argument.

```
docker container logs --tail 2 webserver
```

**NOTE**: A negative number or a non-integer, when passed as an argument to **`--tail`** option is invalid. The value is set to default **`all`** in that case.

5. To list all the available options of **`container inspect`** command use –

```
docker container inspect --help
```

6. To inspect all available states of the container use –

```
# New Way
# docker container inspect <CONTAINER> [CONTAINER …]
docker container inspect webserver

# Old Way
# docker inspect <CONTAINER> [CONTAINER …]
docker inspect webserver
```

7. To get more specific details of the container, use the `--format` option.

```
docker container inspect --format "{{.Config.Image}}" webserver
```

8. To return sub-section of the json use the below command -

```
docker container inspect --format "{{json .Config}}" webserver
```

9. Try the **`grep`** command use –

```
docker container inspect webserver | grep -i Image
```

#### **Activity 08 - Stopping, Starting, Killing, and Removing container**

***

1. For a graceful shutting down of container use the stop command -

```
# New Way
# docker container stop <CONTAINER>
docker container stop webserver

# Old Way
# docker stop <CONTAINER>
docker stop webserver2
```

2. We can also use the **`kill`** command to stop a running container but it will stop the main entry point process abruptly which may lead to a damaged file system.

```
# New Way
# docker container kill <CONTAINER>
docker container kill webserver3

# Old Way
# docker kill <CONTAINER>
```

3. For starting a stopped container use the command –

```
# New Way
# docker container start <CONTAINER>
docker container start webserver

# Old Way
# docker start <CONTAINER>
docker start webserver2
```

4. To start a container and attach it to the client use the **`-a`** option with start command

```
docker container start -a webserver3
```

You can open a new web browser tab and use the IP address along with container port number which is 8888 in our case. That will log the output.

**NOTE**: Use (**`ctrl + c`**) to stop the running container in attach mode.

5. To filter out exited container use **`--filter`** option -

```
docker container ls --filter status=exited
```

6. To remove a stopped container without any error using the command

```
# New Way
# docker container rm <CONTAINER>
docker container rm webserver3

# Old Way
# docker rm <CONTAINER>
docker rm <CONTAINER>
```

7. To remove a running container use the **`-f`** or **`--force`** option -

```
docker container rm --force <CONTAINER_ID|CONTAINER_NAME> 
```

8. To pass the output of one docker command to another docker command use the **`$`** symbol. To remove all the containers running or stopped use -

```
docker container rm -f $(docker container ls -aq)
```

9. Use the **`--rm`** option when creating a new container, which will automatically remove the container when it is stopped.

```
docker container run --detach --rm --name webserver --publish 80:80 nginx:alpine
```

10. To stop the container created earlier and verify use -

```
docker container stop webserver 
# verify after the container is stopped, it is removed because of --rm option used while running a container
docker container ls -a
```

***

#### **Activity 09 - Setting environment variables**

***

1. To set an environment variables that would be used by container use the **`--env`** or **`-e`** option.

```
docker container run --rm \
--env SOME_VAR=ValueWithoutSpaces \
-it alpine sh
```

2. To set multiple environment variables you can use **`--env`** or **`-e`** option multiple times in command -

```
docker container run --rm \
--env SOME_VAR_ONE=ValueOne \
--env SOME_VAR_TWO="Value With Spaces" \
--env SOME_VAR_THREE=ValueThree \
-it alpine sh
```

3. To echo the different variables value set inside container use -

```
echo $SOME_VAR_ONE && echo $SOME_VAR_TWO && echo $SOME_VAR_THREE
```

**NOTE**: The syntax of **`&&`** is used to join multiple commands.

***

#### **Activity 10 - Bind Mounting a container**

***

1. Create a **`my-web-app`** directory and then create a simple HTML file with name as **`index.html`**.

```
mkdir ~/my-web-app
vim ~/my-web-app/index.html
```

index.html content below -

```
<html>
    <body>
        <h1>ATCI : Working with Containers!</h1>
    </body>
</html>
```

2. The **`--volume`** or **`-v`** option is used to bind mount the app directory to web server application.

```
--volume <HOST-PATH> : <CONTAINER-PATH> : [<OTHER-OPTIONS>]
```

```
docker container run --rm --detach --name webserver --publish 80:80 \
--volume $(pwd)/my-web-app:/usr/share/nginx/html:ro \
nginx:alpine
```

**NOTE**: we have used **`ro`** for _`readonly`_ purpose

3. Open a new web browser tab and type the IP address on the address bar to see the content of your **`index.html`** file that gets mounted inside container html directory.
4. Starting from docker **`17.06`** a new option i.e. **`--mount`** is preferred over **`--volume`** option.

```
--mount type=(bind|volume|tmpfs),source=HOST-PATH,destination=CONTAINER-PATH,[options]
OR
--mount type=(bind|volume|tmpfs),source=HOST-PATH,target=CONTAINER-PATH,[options]
OR
--mount type=(bind|volume|tmpfs),src=HOST-PATH,dst=CONTAINER-PATH,[options]
```

```
docker container run --rm --detach --name anotherwebserver --publish 8888:80 \
--mount type=bind,src=$(pwd)/my-web-app,dst=/usr/share/nginx/html,readonly \
nginx:alpine
```

5. Open a new web browser tab and type the IP address on the address bar, use the port number 8888 to access the modified **`index.html`** page.
6. Try updating the file in **`my-web-app`** directory and refresh the browser window to see modified changes.
7. To remove all the containers use -

```
docker container rm -f $(docker container ls -aq)
```

***

#### **Activity 11 - Volume mounting a container**

***

1. To see a host of available sub commands of **`volume`** management command use -

```
docker volume --help
```

2. To create a new volume managed by docker use -

```
# docker volume create <VOLUME>
docker volume create my-alpine-volume
```

3. To list the volume created use the command –

```
docker volume ls
```

4. To inspect the volume use the command -

```
# docker volume inspect <VOLUME>
docker volume inspect my-alpine-volume
```

5. To add this volume to a container use the below command –

To mount the volume **`my-alpine-volume`** into **`/custom_directory`** in the container that could be access using the exec command.

```
docker container run --name alpine-one -it \
--mount source=my-alpine-volume,target=/custom_directory \
alpine sh
```

**NOTE**: The **`custom_directory`** is created automatically inside the container to mount the volume.

6. To add a new file inside the **`custom_directory`** use touch command -

```
touch custom_directory/sample.txt
```

7. To list the content of **`custom_directory`** use the command below, then exit using (**`ctrl + p + q`**) -

```
ls -al custom_directory
```

8. Create another container that would be mounted on the same volume and we can see the same file inside this new container.

```
docker container run --name alpine-two -it \
--mount source=my-alpine-volume,target=/my_directory \
alpine sh
```

9. To view the file created in alpine-one container inside **`my_directory`** use -

```
ls -al my_directory
```

10. Add a new file from this running container, say **`another_sample.txt`** inside the **`my_directory`**

```
touch my_directory/another_sample.txt
```

11. To come out of the container and keep it running use (**`ctrl + p + q`**)
12. Now, exec into the alpine-one container and check the content of the **`custom_directory`**

```
docker container exec -it alpine-one sh
ls -al custom_directory
```

13. To remove a docker volume use the below command -

```
# docker volume rm <volume-name>
docker volume rm my-alpine-volume
```

**NOTE**: If you try to remove the volume which is already attached to the container it will throw the error.

14. First remove all the containers attached to the volume we have to remove –

```
docker container rm -f $(docker container ls -aq)
docker volume rm my-alpine-volume
```

**NOTE**: If a container is started with a volume that doesn't exist, docker will automatically create that volume.

***

#### **Activity 12 - Creating a network and assigning it to an existing container**

***

1. To create two containers communicating with each other use -

```
docker container run --detach --rm --name alpine-one alpine sleep 3600
docker container run --detach --rm --name alpine-two alpine sleep 3600
```

2. To check the status of the container running use -

```
docker container ls
```

3. To ping from alpine-one container to alpine-two container just by using container name use -

```
docker container exec -it alpine-one ping -c 4 alpine-two
```

4. To see the list of available sub-command use with **`network`** management command use -

```
docker network --help
```

5. To create a new network using the bridge driver use the below command -

```
# docker network create --driver bridge <NETWORK-NAME>
docker network create --driver bridge my-network
```

6. To see the list of available networks use -

```
docker network ls
```

7. To connect the existing container to a network use -

```
docker network connect my-network alpine-one
docker network connect my-network alpine-two
```

8. To inspect a network use –

```
# docker network inspect <NETWORK-NAME>
docker network inspect my-network
```

**NOTE**: Check that both the containers are connected to this network

9. Now, try to ping from alpine-one container to alpine-two container once again -

```
docker container exec -it alpine-one ping -c 4 alpine-two
```

10. To disconnect an existing container from a network use **`disconnect`** command -

```
docker network disconnect my-network alpine-one
docker network disconnect my-network alpine-two
docker network inspect my-network
```

11. To remove a network use the command -

```
# docker network rm <NETWORK-NAME>
docker network rm my-network
```

12. To delete the containers created use -

```
docker container rm -f $(docker container ls -aq)
```

***

#### **Case Study - Creating a MySQL server and connecting to it using Adminer (a web based UI to communicate with MySQL Server)**

***

In this case study, we need to set up a new mysql server and using a web ui we should be able to execute sql query on the mysql server.

**HINTS**:

1. Look for the official MySQL image on hub.docker.com. Read the description to understand the different environment variables.
2. To store the data generated by mysql create a volume and mount it to container.
3. Look for the official adminer image on hub.docker.com. Read the description to understand the different environment variables. Look for the specific environment variables to set the name of mysql server.
4. Create a separate network to join the `adminer` and `mysql` server for communication.

***

***

#### **Activity 13 - Commit Changes in a Running Container**

***

1. To run a nginx webserver inside a container use -

```
docker container run --rm --detach --name webserver -p 80:80 nginx:alpine
```

2. Open a web browser and access the default web page for nginx webserver, using the private IP address
3. To exec into the webserver container use -

```
docker container exec -it webserver sh
```

4. Open the **`index.html`** file, which is located in the path given below -

```
vi /usr/share/nginx/html/index.html
```

and edit it like

```
<body>
    <h1>ATCI - Working with Containers Training!</h1>
</body>
```

**NOTE**:

use the key **`I`** to be in insert mode to edit the file

use esc with **`:wq`** to write and quit from vi

5. After changes, to exit out of the shell use the **`exit`** command. Refresh the browser tab to see the modified changes.
6. To list all the options available with **`commit`** command use -

```
docker container commit --help
```

7. To commit changes, and create a new image from a container use:

```
# New Way
docker container commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

docker container commit webserver mynginx:1.0

# Old Way
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

8. To view the image that is just created use -

```
docker image ls
```

9. Run a new container webservermynginx with the from the **`mynginx:1.0`** image

```
docker container run --rm --detach --name webservermynginx -p 8080:80 mynginx:1.0
```

10. Open a web browser and access the default web page for webservermynginx, using the private IP address
11. To exec into the webservermynginx container use -

```
docker container exec -it webservermynginx sh
```

12. Open the **`index.html`** file to see, which is located in the path given below -

```
cat /usr/share/nginx/html/index.html
```

Can see the below configuration:

```
<body>
    <h1>ATCI - Working with Containers Training!</h1>
</body>
```

13. To stop the container use -

```
docker container stop webserver
docker container stop webservermynginx
```
