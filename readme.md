# Download and run nexus on a server and Push docker images to nexus and fetch docker images from nexus (Run nexus as a container on server)

In this repo i combined the steps to download and run nexus on a server ( of ubuntu os) and aslo how to push docker image to nexus and fecth docker images from nexus ans also run docker as a container on server

---

```bash

sudo apt update # update before installing any new softwares

sudo apt install openjdk-11-jdk -y # install java before installing nexus

#install nexus now using wget

sudo wget https://download.sonatype.com/nexus/3/nexus-3.79.1-04-linux-x86_64.tar.gz

# untar the nexus file

sudo tar -zxvf nexus-3.79.1-04-linux-x86_64.tar.gz
```
---

```bash

# add nexus user to the users of ubuntu os it is best practice to run nexus as a nexus user

sudo adduser nexus

#change permissions for the nexus folders

sudo chown -R nexus:nexus nexus-3.79.1-04
sudo chown -R nexus:nexus sonatype-work

# go inside this foledr and edit necus.rc file nexus-3.79.1-04/bin/nexus.rc
cd nexus-3.79.1-04/bin/
vim nexus.rc
```

```vim
run_as_user="nexus"
```

```bash

#switch to nexus user
su - nexus

#start nexus 
/opt/nexus-3.79.1-04/bin/nexus start

#open firewall rule on port 8081 and connect it ipaddress:8081

#check password for nexus on /opt/sonatype-work/nexus3/admin.password
cat /opt/sonatype-work/nexus3/admin.password
```
---
---

## Push docker images to nexus and fetch docker images from nexus

1) Create a Repository in nexus (example : docker-hosted-myapp)

2) In order to start pushing the docker images to docker repository, we first need to login to nexus using a nexus user which has access to docker hosted repository
    . create a role that has privilege for access to docker hosted repository i.e the repo you pushing the image

3)  Give permission for the user the docker role you just created

4) configure a port for connecting to the repo as it can connect through the ipaddress:8081/repository/repo_name

5) allow the port connection (just done above) in firewall connection of the vm

6) Realms --> docker login we get token of authentication of docker repo from nexus to docker client and token is stored in /docker.config.json will include all the authentication i made to all the different docker repositories.

7) last thing is to do in docker-desktop configure docker client to allow our docker registory to allow as insecurity registry 
    - in Linux : /etc/docker/daemon.json
    - in docker-desktop : { "insecure-registries": ["ipaddress:8083"] }

8) docker login in your local machine 
    - docker login ipaddress:portnumber(configured for connecting in step 	the credentials here needs to be the necus users credentials

9) build the image and re tag it
    - docker tag my-app:1.0 <ipaddress>:<port of repo>/my-app:1.0

10) Now push the image with complete name
    - docker push <ipaddress>:<port of repo>/my-app:1.0

11) now we can see the image in the nexus browse tab under repositories

12) can fetch from nexus
    - curl -u nexus-user:nexus-password -X GET 'http:/ipaddress:port/service/rest/v1/components?repository=<Repo_name>

---
---

## run docker as a container on server

- create a server

- update 
	- sudo pt update

- install docker suing snap
	- sudo snap install docker

- look for nexus3 image in docker hub

- when we start nexus we need to configure volume for it for persistence of data by running it with mounted volume
	- docker volume create --name nexus-data
	- docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3

- check the running port whether nexus is running or not
    - netstat -lnpt
	- if not have netstat install it --> sudo apt install net-tools
