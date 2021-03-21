Run tomcat container

`git clone https://github.com/softwareyoga/docker-tomcat-tutorial.git`{{execute}} 

`cd docker-tomcat-tutorial`{{execute}} 

`docker build -t mywebapp .`{{execute}} 

`clear`{{execute}}

`docker run -d -p 80:8080 mywebapp`{{execute}}

Run Python application container

`git clone https://github.com/CiscoIOx/docker-demo-apps.git -b master`{{execute}} 

`cd docker-demo-apps/simple-python-app`{{execute}} 

`docker build -t iox-simple-py:1.0 .`{{execute}} 

`clear`{{execute}}

`docker run -d --name iox-simple-py iox-simple-py:1.0`{{execute}}

Run Konex AC Container

`docker pull anugiri86/amxce_ac:1.0`{{execute}} 

`docker run -d -p 8087:8087 --name amxce_ac anugiri86/amxce_ac:1.0`{{execute}}

`clear`{{execute}}

List the containers

`docker ps`{{execute}}

Container Images and Build File Best Practices:

1.Create a user for the container

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: User={{	.Config.User }}'`{{execute}}

`clear`{{execute}}

2.Add HEALTHCHECK instruction to the container image

`docker inspect --format='{{ .Config.Healthcheck }}' anugiri86/amxce_ac:1.0`{{execute}}

3.Use COPY instead of ADD in Dockerfile

`docker history anugiri86/amxce_ac:1.0`{{execute}}

4.Do not use privileged containers

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: Privileged={{.HostConfig.Privileged }}'`{{execute}}

`clear`{{execute}}

5.Do not mount sensitive host system directories (/etc,/sys,/usr) on containers 

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: Volumes={{ .Mounts}}'`{{execute}}

6.Do not map privileged ports within containers

`docker ps --quiet | xargs docker inspect --format '{{ .Id }}: Ports={{.NetworkSettings.Ports }}'`{{execute}}

`clear`{{execute}}

7.Limit memory usage for container

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: Memory={{.HostConfig.Memory }}'`{{execute}}

8.Mount container's root filesystem as read only

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }} ReadonlyRootfs={{.HostConfig.ReadonlyRootfs }}'`{{execute}}

`clear`{{execute}}


Build the second image from Dockerfile.

`docker build -t acme/my-final-image:1.0 -f Dockerfile .`{{execute}}


Check out the sizes of the images:

`docker image ls`{{execute}}


Check out the layers that comprise each image:

`docker history acme/my-base-image:1.0`{{execute}}


`docker history acme/my-final-image:1.0`{{execute}}


Notice that all the layers are identical except the top layer of the second image. 
All the other layers are shared between the two images, and are only stored once in /var/lib/docker/. 
The new layer actually doesn’t take any room at all, because it is not changing any files, but only running a command.





