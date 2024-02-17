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

## NGROK

> docker run -it -e NGROK_AUTHTOKEN=\<token> ngrok/ngrok **http 80**
>
> El **http 80** reemplazar por ejemplo con **tcp \<ip>:\<puerto>**
>
> Para obtener el \<token> ingresar a ====>  https://dashboard.ngrok.com/login 

## RABBIT MQ

> docker run -d --hostname rabbit --name rabbit-server -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-management
>
> El puerto **15672** es del UI y el puero **5672** es del rabbit broker
>
> Para ingresar user/password ====>  http://localhost:15672

## WSO2 API Manager

> docker run --name api-manager -p 8280:8280 -p 8243:8243 -p 9443:9443 wso2/wso2am:4.0.0
>
> https://localhost:9443/{service-name}
>
> service-name => [carbon, publisher, devportal]
>
> **user/pass** => admin/admin
> 
> ====> https://blog.programster.org/deploy-wso2-api-manager-with-docker

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

## MONGO

> docker pull mongo
> 
> docker run -d --name mongodb -p 27017:27017 mongo
> 
> docker exec –it mongodb mongosh
>
> docker exec -it e884ebef3f36 mongosh
> 
> docker inspect 32f4175e105b
> 
> user:admin
> 
> password:password
> 
> port:27017


## POSTGRES

> docker pull postgres
> 
> docker run --name myPostgresDb -p 5455:5432 -e POSTGRES_USER=postgresUser -e POSTGRES_PASSWORD=postgresPW -e POSTGRES_DB=postgresDB -d postgres
> 
> docker inspect 77073c8e7c7d  ---> **Permite obtener el IP**
> 
> docker container ls 
> 
> docker stop 77073c8e7c7d
> 
> docker start 77073c8e7c7d
> 
> docker rm -f 77073c8e7c7d
> 
> docker image rm -f 77073c8e7c7d
> 
> docker exec -it myPostgresDb bash
> 

> docker pull dpage/pgadmin4
> 
> docker run -p 80:80 -e 'PGADMIN_DEFAULT_EMAIL=sainthilde@gmail.com' -e 'PGADMIN_DEFAULT_PASSWORD=postgresPW' -d dpage/pgadmin4
>
> http://localhost/browser/
> 
> connect params: 172.17.0.2 + credentials
> 

## SQL SERVER 2019

> docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Suprime2252" -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
> 
> connect params: user=sa password=Suprime2252

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
