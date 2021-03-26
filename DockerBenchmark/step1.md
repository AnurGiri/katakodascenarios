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

Run Bookstore application container on TEA Server

`docker pull anugiri86/book:v1`{{execute}} 

`docker network create amxce-net`{{execute}} 

`docker run -d -p 7777:7777 --name bookstore --network amxce-net anugiri86/book:v1`{{execute}} 

`clear`{{execute}}

`docker pull anugiri86/tea240:v10`{{execute}} 

`docker pull anugiri86/amxceteaagent:1.0`{{execute}} 

`docker run -d -p 8777:8777 --name teaserver -h teaserver --network amxce-net anugiri86/tea240:v10`{{execute}} 

`docker run -d --name teaagent -h teaagent --network amxce-net -e tea_server_url=http://teaserver:8777/tea -e amxce_containers_list=bookstore anugiri86/amxceteaagent:1.0`{{execute}} 

Run Konex AC Container

`docker pull anugiri86/amxce_ac:1.0`{{execute}} 

`docker run -d -p 8087:8087 --name amxce_ac anugiri86/amxce_ac:1.0`{{execute}}

`clear`{{execute}}

List the containers

`docker ps`{{execute}}

Best Practices: Container Images and Build File 

1.Create a user for the container

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: User={{	.Config.User }}'`{{execute}}

`clear`{{execute}}

2.Add HEALTHCHECK instruction to the container image

`docker inspect --format='{{ .Config.Healthcheck }}' anugiri86/amxce_ac:1.0`{{execute}}

3.Use COPY instead of ADD in Dockerfile

`docker history anugiri86/amxce_ac:1.0`{{execute}}

`clear`{{execute}}

`docker history anugiri86/tea240:v10`{{execute}}

`clear`{{execute}}

`docker history anugiri86/amxceteaagent:1.0`{{execute}}

`clear`{{execute}}

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

8.Set container CPU priority appropriately

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}: CpuShares={{.HostConfig.CpuShares }}''`{{execute}}

9.Mount container's root filesystem as read only

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }} ReadonlyRootfs={{.HostConfig.ReadonlyRootfs }}'`{{execute}}

`clear`{{execute}}

10.Set the 'on-failure' container restart policy to 5

`docker ps --quiet --all | xargs docker inspect --format '{{ .Id }}:RestartPolicyName={{ .HostConfig.RestartPolicy.Name }} MaximumRetryCount={{.HostConfig.RestartPolicy.MaximumRetryCount }}''`{{execute}}

`clear`{{execute}}

