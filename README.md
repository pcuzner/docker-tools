#docker-tools

##Introduction
By default docker containers get a dynamic IP address when they are instantiated. This can make things a little convoluted when you want to connect to the container with ssh for example.  
i.e  
1. Start the container (`docker run`)  
2. Grab the IP address assigned to the container (`docker inspect` & `grep`)  
3. `ssh` to the container using the above IP address  
  
Life's too short, so if you're lazy like me, here's some tools that may make things a little easier  
* **`docker-ip`** .... get the IP address of a running container (from it's name or container id)  
* **`docker-ssh`** ... ssh to a given container (by name or id)  

Here's an example;  
  
```
[paul@rhlaptop ~]$ rpm -q docker-engine  
docker-engine-1.7.0-1.fc22.x86_64  
[paul@rhlaptop ~]$ sudo docker images  
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE  
el7build            latest              1bdbf5b58c91        2 days ago          262.7 MB  
el6build            latest              16db9fba6f26        2 days ago          261.2 MB  
f21build            latest              24d6372a2148        2 days ago          351.3 MB  
f22build            latest              1b2faeba1c5b        2 days ago          380.9 MB  
ubuntu              latest              d2a0ecffe6fa        3 days ago          188.3 MB  
docker.io/centos    latest              7322fbe74aa5        3 weeks ago         172.2 MB  
centos              latest              7322fbe74aa5        3 weeks ago         172.2 MB  
fedora              22                  ded7cd95e059        6 weeks ago         186.5 MB  
docker.io/fedora    22                  ded7cd95e059        6 weeks ago         186.5 MB  
fedora              21                  e26efd418c48        8 weeks ago         241.3 MB  
docker.io/fedora    21                  e26efd418c48        8 weeks ago         241.3 MB  
docker.io/centos    6.6                 8b44529354f3        11 weeks ago        202.6 MB  
centos              6.6                 8b44529354f3        11 weeks ago        202.6 MB  
[paul@rhlaptop ~]$ sudo docker run -d --name=el7build -h el7build el7build  
cd928f9b9ad905b288e63e0b15870dd9374f9cff6b13219e9e6a81702440d6e0  
[paul@rhlaptop ~]$ sudo docker ps  
CONTAINER ID        IMAGE               COMMAND               CREATED             STATUS              PORTS               NAMES  
cd928f9b9ad9        el7build            "/usr/sbin/sshd -D"   4 seconds ago       Up 3 seconds        22/tcp              el7build              
[paul@rhlaptop ~]$ docker-ip el7build  
172.17.0.194  
[paul@rhlaptop ~]$ docker-ssh build@el7build  
# 172.17.0.194:22 SSH-2.0-OpenSSH_6.6.1  
# 172.17.0.194:22 SSH-2.0-OpenSSH_6.6.1  
# 172.17.0.194:22 SSH-2.0-OpenSSH_6.6.1  
[build@el7build ~]$   
  
```
  
##Installation  
The two tools are provided by a single bash script, so the easiest thing to do is just source it from you .bashrc file, like this;  

1. copy the docker-bashrc file to `~/bin` (assuming you have a personal bin directory - if not `mkdir` one!)  

2. Update your `.bashrc` file to include (i.e. source) the docker-bashrc file  
e.g. Add the following entry to ~/.bashrc
```
# User specific aliases and functions
. ~/bin/docker-bashrc
```  

Done..nothing too complicated!  

