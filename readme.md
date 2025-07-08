# Download and run nexus on a server

In this repo i combined the steps to download and run nexus on a server ( of ubuntu os)

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


