# INSTALL DOCKER

## Utilitarios

> yum clean all
> 
> sudo yum install -y yum-utils device-mapper-persistent-data lvm2

## Agregar el repo de docker

> sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

## Instalar

> sudo yum install docker-ce -y
>
> sudo yum install docker-ce -y --allowerasing

## Iniciarlo con el sistema

> sudo systemctl enable docker

## Agregar usuario al grupo docker 

> whoami ====> Saber el nombre de tu usuario
> 
> sudo usermod -aG docker <nombre_de_salida_en_whoami>

# START / STOP SERVICE

> sudo systemctl start docker
> 
> sudo systemctl stop docker

## APACHE TOMCAT

> docker run -d --name tomcat9 -p 8888:8080 tomcat:9.0
> 
> //docker exec -u root -ti 7e922491f175 bash
> 
> docker exec -it 7e922491f175 /bin/bash
> 
> mv webapps webapps2
> 
> mv webapps.dist/ webapps
> 
> docker stop 7e922491f175
> 
> docker start 7e922491f175
> 
> docker rm -f 7e922491f175
>
> ====> https://www.topzenith.com/2020/07/http-status-404-not-found-docker-tomcat-image.html

## MYSQL

> docker run -d -p 3333:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:latest
> 
> docker stop 7dcbc2811678
> 
> docker start 7dcbc2811678
> 
> docker rm -f 7dcbc2811678
> 
> user:root
> 
> password:123456
> 
> port:3333

## WebSphere MQ

> docker run --env LICENSE=accept --env MQ_QMGR_NAME=QM1 --publish 1414:1414 --publish 9443:9443 --detach ibmcom/mq
> 
> docker exec --tty --interactive b1f85ef367f3 dspmq
> 
> docker exec -it b1f85ef367f3 dspmq
> 
> docker stop b1f85ef367f3
> 
> docker start b1f85ef367f3
> 
> docker rm -f b1f85ef367f3

## Integration Bus

> docker run --name myNode -e LICENSE=accept -e NODENAME=MYNODE -e SERVERNAME=MYSERVER -P ibmcom/iib
> 
> docker exec -it <container name> /bin/bash
>   
> docker stop 7dcbc2811678
>   
> docker start 7dcbc2811678
>   
> docker rm -f 7dcbc2811678

## Hashicorp Vault
  
> docker run --cap-add=IPC_LOCK -e 'VAULT_DEV_ROOT_TOKEN_ID=myroot' -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:1234' -p 8200:1234 vault
> 
> ====> http://127.0.0.1:8200/ui/
