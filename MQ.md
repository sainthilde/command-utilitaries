## crear queue manager	
>
> crtmqm -q -d MY.DEFAULT.XMIT.QUEUE -u SYSTEM.DEAD.LETTER.QUEUE  SATURN.QUEUE.MANAGER

* -q 	gestor de colas predeterminado
* -d 	cola de transmisión predeterminada 
* -u 	la cola de mensajes no entregados predeterminada 
* SATURN.QUEUE.MANAGER	nombre de este gestor de colas

##	iniciar queue manager	
> 
> strmqm SATURN.QUEUE.MANAGER

##	detener queue manager	
> 
> endmqm SATURN.QUEUE.MANAGER

### Conclusión progresiva
Por omisión, el mandato endmqm realiza una conclusión progresiva 
> 
> endmqm -c SATURN.QUEUE.MANAGER   
* comunica a las aplicaciones que deben detenerse
* no recibirá ninguna notificación cuando se hayan detenido todas las aplicaciones
* es equivalente a un mandato endmqm SATURN.QUEUE.MANAGER
> 
> endmqm -w SATURN.QUEUE.MANAGER
* el mandato esperará hasta que todas las aplicaciones se hayan detenido y el gestor de colas haya finalizado

### Conclusión inmediata
En una conclusión inmediata, se permite que terminen todas las llamadas MQI actuales pero cualquier llamada nueva fallará. 
Este tipo de conclusión no espera a que las aplicaciones se desconecten del gestor de colas
> 
> endmqm -i SATURN.QUEUE.MANAGER   

### Conclusión preferente
Si una conclusión inmediata no funciona, deberá recurrir a una conclusión preferente
> 
> endmqm -p SATURN.QUEUE.MANAGER

### Reiniciar un queue manager 
> 
> strmqm SATURN.QUEUE.MANAGER   

### Suprimir un queue manager  
> 
> dltmqm SATURN.QUEUE.MANAGER   
* suprimen todos los recursos asociados al mismo

### visualizar queue manager creados
utilizando el siguiente mandato para visualizar todos los gestores de colas
> 
> dspmq

------------

#######################################
##### 	mandatos MQSC  		 ######
#######################################

runmqsc
==> Como en este mandato no se ha especificado un nombre de gestor de colas, 
==> los mandatos MQSC los procesa el gestor de colas predeterminado

runmqsc SATURN.QUEUE.MANAGER

#######################################
#mandatos MQSC desde archivos de texto#
#######################################

runmqsc < myprog.in
runmqsc SATURN.QUEUE.MANAGER < myprog.in
==>ejecuta una secuencia de mandatos contenida en el archivo de texto myprog.in

runmqsc < myprog.in > myprog.out
runmqsc SATURN.QUEUE.MANAGER < myprog.in > myprog.out


##### runmqsc para verificar mandatos #########

Para ello, establezca el indicador -v en el mandato runmqsc

runmqsc -v < myprog.in > myprog.out
runmqsc -v jupiter.queue.manager < myprog.in > myprog.out

runmqsc -w 30 -v jupiter.queue.manager < myprog.in > myprog.out
==> No puede utilizar este método para verificar los mandatos MQSC de forma remota
==> el indicador -w, que se utiliza para indicar que el gestor de colas es remoto, 
==> se ignora y el mandato se ejecuta localmente en modalidad de verificación




##########################################################
##### Visualización de atributos de un queue manager #####
##########################################################

1ero runmqsc SATURN.QUEUE.MANAGER
2do  DISPLAY QMGR


##########################################################
##### modificacion de atributos de un queue manager  #####
##########################################################

1ero runmqsc SATURN.QUEUE.MANAGER
2do  ALTER QMGR DEADQ (ANOTHERDLQ) INHIBTEV (ENABLED)


#######################################
###  Definición de una cola local   ###
#######################################


El nombre de la cola local por omisión es SYSTEM.DEFAULT.LOCAL.QUEUE  se ha creado durante la instalación 

DEFINE QLOCAL (ORANGE.LOCAL.QUEUE) 
==> sus atributos se copian del SYSTEM.DEFAULT.LOCAL.QUEUE

DEFINE QLOCAL (ORANGE.LOCAL.QUEUE) +
              DESCR('Cola para mensajes de otros sistemas') +
              PUT(ENABLED) +
              GET (ENABLED) +
              NOTRIGGER +
              MSGDLVSQ (PRIORITY) +
              MAXDEPTH(5000) +
              MAXMSGL (4194304) +
      	      USAGE (NORMAL);
==> tambien se pueden crear con ciertos atributos


##########################################################
#### Definición de una cola de mensajes no entregados ####
##########################################################


crtmqm -u DEAD.LETTER.QUEUE SATURN.QUEUE.MANAGER
ALTER QMGR DEADQ (ANOTHERDLQ)

Una cola de mensajes no entregados no tiene ningún requisito especial excepto que: 
==>Debe ser una cola local 
==>Su atributo MAXMSGL (longitud máxima de mensajes) debe permitir que la cola 
==>pueda alojar los mensajes más grandes que el gestor de colas tenga que manejar 
==>más el tamaño de la cabecera de mensaje no entregado (MQDLH) 




#######################################
### visualizacion de una cola local ###
#######################################

DISPLAY QUEUE (SYSTEM.DEFAULT.LOCAL.QUEUE)
DISPLAY QUEUE (SYSTEM.*)
DISPLAY QUEUE (ORANGE.LOCAL.QUEUE) +
        MAXDEPTH +
        MAXMSGL +
        CURDEPTH;


#######################################
#Copia de una definición de cola local#
#######################################

DEFINE QLOCAL (MAGENTA.QUEUE) +
       LIKE (ORANGE.LOCAL.QUEUE)
DEFINE QLOCAL (THIRD.QUEUE) +
       LIKE (ORANGE.LOCAL.QUEUE) +
       MAXMSGL(1024);

==>Cuando se utiliza el atributo LIKE en un mandato DEFINE, 
==>sólo se copian los atributos de la cola. No se copian los mensajes existente en la cola. 

==>Si define una cola local sin especificar LIKE, 
==>es lo mismo que DEFINE LIKE(SYSTEM.DEFAULT.LOCAL.QUEUE). 


################################################
## Modificación de atributos de colas locales ##
################################################


ALTER QLOCAL (ORANGE.LOCAL.QUEUE) MAXMSGL(10000)
DEFINE QLOCAL (ORANGE.LOCAL.QUEUE) MAXMSGL(10000) REPLACE



#######################################
###### 	    limpiar una cola 	 ######
#######################################


CLEAR QLOCAL (MAGENTA.QUEUE)


#######################################
##### Supresión de una cola local #####
#######################################


DELETE QLOCAL (PINK.QUEUE) 

DELETE QLOCAL (PINK.QUEUE) PURGE
==>si la cola tiene uno o más mensajes confirmados y ningún mensaje sin confirmar, 
==>se puede suprimir sólo si se especifica la opción PURGE

DELETE QLOCAL (PINK.QUEUE) NOPURGE
==>Especificar NOPURGE en lugar de PURGE asegura que la cola no se suprime 
==>si contiene algún mensaje confirmado


#######################################
#### Definición de una cola alias  #### 
#######################################


DEFINE QALIAS (ALPHAS.ALIAS.QUEUE) +
       TARGQ (YELLOW.QUEUE) +
       PUT (ENABLED) +
       GET (DISABLED)

DEFINE QALIAS (BETAS.ALIAS.QUEUE) +
       TARGQ (YELLOW.QUEUE) +
       PUT (DISABLED) +
       GET (ENABLED)


==>puede utilizar colas alias para que hacer que una sola cola (la cola destino) 
==>parezca tener atributos distintos para aplicaciones distintas. 
==>Esto se hace definiendo dos alias, uno para cada aplicación. Suponga que tiene dos aplicaciones: 

•La aplicación ALPHA puede colocar mensajes en YELLOW.QUEUE, 
pero no tiene autorización para obtener mensajes de dicha cola. 
•La aplicación BETA puede obtener mensajes de YELLOW.QUEUE, 
pero no tiene autorización para colocar mensajes en ella. 



#######################################
###### mandatos con colas alias  ###### 
#######################################


DISPLAY QUEUE (ALPHAS.ALIAS.QUEUE)

ALTER QALIAS (ALPHAS.ALIAS.QUEUE) TARGQ(ORANGE.LOCAL.QUEUE) FORCE
==>Utilice el mandato siguiente para alterar el nombre de cola base, 
==>en el que se resuelve el alias, donde la opción force fuerza el cambio 
==>incluso si la cola está abierta

DELETE QALIAS (ALPHAS.ALIAS.QUEUE)
==>No se puede suprimir una cola alias si una aplicación tiene actualmente abierta la cola


#######################################
#### Definición de una cola modelo #### 
#######################################

DEFINE QMODEL (GREEN.MODEL.QUEUE) +
              DESCR('Cola para mensajes de la aplicación X') +
              PUT (DISABLED) +
              GET (ENABLED) +
              NOTRIGGER +
              MSGDLVSQ (FIFO) +
              MAXDEPTH (1000) +
              MAXMSGL (2000) +
              USAGE(NORMAL) +
              DEFTYPE (PERMDYN)



==>Desde el atributo DEFTYPE, puede ver que las colas reales creadas con esta plantilla son colas dinámicas permanentes.
==>Todos los atributos no especificados se copian automáticamente de la cola predeterminada SYSTEM.DEFAULT.MODEL.QUEUE.
==>Puede utilizar los atributos LIKE y REPLACE cuando defina colas modelo, del mismo modo que los utiliza con colas locales


#######################################
## otros mandatos en una cola modelo ##
#######################################

DISPLAY QUEUE (GREEN.MODEL.QUEUE)
ALTER QMODEL (BLUE.MODEL.QUEUE) PUT(ENABLED
DELETE QMODEL (RED.MODEL.QUEUE)



################################################################
####### Utilización de un objeto de servicio de servidor #######
################################################################

runmqsc SATURN.QUEUE.MANAGER

DEFINE SERVICE(S1) +
       CONTROL(QMGR) +
       SERVTYPE(SERVER) +
       STARTCMD('+MQ_INSTALL_PATH+ bin/runmqtrm') +
       STARTARG('-m +QMNAME+ -q ACCOUNTS.INITIATION.QUEUE') +
       STOPCMD('+MQ_INSTALL_PATH+ bin/amqsstop') +
       STOPARG('-m +QMNAME+ -p +MQ_SERVER_PID+')



==> +MQ_INSTALL_PATH+ es una señal que representa al directorio de instalación. 
==> +QMNAME+ es una señal que representa al nombre del gestor de colas. 
==> ACCOUNTS.INITIATION.QUEUE es la cola de inicio. 
==> amqsstop es un programa de ejemplo que se proporciona con WebSphere MQ 
    y que solicita al gestor de colas que interrumpa todas las conexiones para el ID de proceso. 
    amqsstop genera mandatos en PCF, por lo que el servidor de mandatos debe estar en ejecución. 
==> +MQ_SERVER_PID+ es una señal que representa el ID de proceso que se pasa al programa de detención


START SERVICE(S1)

DISPLAY SVSTATUS(S1)

ALTER SERVICE(S1) +
      STARTARG('-m +QMNAME+ -q JUPITER.INITIATION.QUEUE')

STOP SERVICE(S1)



################################################################
##### Definición de una cola de aplicación para activación #####
################################################################


DEFINE QLOCAL (MOTOR.INSURANCE.QUEUE) +
       PROCESS (MOTOR.INSURANCE.QUOTE.PROCESS) +
       MAXMSGL (2000) +
       DEFPSIST (YES) +
       INITQ (MOTOR.INS.INIT.QUEUE) +
       TRIGGER +
       TRIGTYPE (DEPTH) +
       TRIGDPTH (100)+
       TRIGMPRI (5)



==> QLOCAL (MOTOR.INSURANCE.QUEUE) 
==> Es el nombre de la cola de aplicación que se define.
 
==> PROCESS (MOTOR.INSURANCE.QUOTE.PROCESS) 
==> Es el nombre de la definición de proceso que describe la aplicación 
    que se va a iniciar mediante un programa supervisor desencadenante. 

==> MAXMSGL (2000) 
==> Especifica la longitud máxima de los mensajes de la cola. 

==> DEFPSIST (YES) 
==> Especifica que los mensajes de esta cola son persistentes de forma predeterminada. 

==> INITQ (MOTOR.INS.INIT.QUEUE) 
==> Es el nombre de la cola de inicio en la que el gestor de colas va a colocar el mensaje desencadenante. 

==> TRIGGER 
==> Es el valor del atributo de desencadenamiento. 

==> TRIGTYPE (DEPTH) 
==> Especifica que se genera un suceso desencadenante cuando el número de mensajes de la prioridad obligatoria 
    (TRIGMPRI) alcanza el número especificado en TRIGDPTH. 

==> TRIGDPTH (100) 
==> Especifica el número de mensajes necesarios para generar un suceso desencadenante. 

==> TRIGMPRI (5) 
==> Es la prioridad de los mensajes que el gestor de colas va a incluir en el recuento para decidir 
    si va a generar un suceso desencadenante. Sólo se incluyen en el recuento los mensajes con prioridad 5 o superior. 




#######################################
## Definición de una cola de inicio ###
#######################################


DEFINE QLOCAL(MOTOR.INS.INIT.QUEUE) +
       GET (ENABLED) +
       NOSHARE +
       NOTRIGGER +
       MAXMSGL (2000) +
       MAXDEPTH (1000)



#######################################
##### Definición de un proceso ########
#######################################


DEFINE PROCESS (MOTOR.INSURANCE.QUOTE.PROCESS) +
               DESCR ('Proceso de mensaje de petición de seguro') +
               APPLTYPE (UNIX) +
               APPLICID ('/u/admin/test/IRMP01') +
               USERDATA ('open, close, 235')


==> MOTOR.INSURANCE.QUOTE.PROCESS 
==> Es el nombre de la definición de proceso. 

==> DESCR ('Proceso de mensaje de petición de seguro') 
==> Describe el programa de aplicación relacionado con esta definición. 
    Este texto se visualiza cuando utiliza el mandato DISPLAY PROCESS 
    y sirve para ayudarle a identificar lo que hace el proceso. 
    Si utiliza espacios en la serie de caracteres, debe encerrar la serie entre comillas simples. 

==> APPLTYPE (UNIX) 
==> Es el tipo de aplicación que debe iniciarse. 

==> APPLICID ('/u/admin/test/IRMP01') 
==> Es el nombre del archivo ejecutable de la aplicación, especificado como nombre de archivo totalmente calificado. 
    En sistemas Windows®, un valor típico de APPLICID sería c:\appl\test\irmp01.exe. 

==> USERDATA ('open, close, 235') 
==> Son datos definidos por el usuario, que la aplicación puede utilizar. 




################################################################
### Visualización de atributos de una definición de proceso  ###
################################################################


DISPLAY PROCESS (MOTOR.INSURANCE.QUOTE.PROCESS)

ALTER PROCESS 

DELETE PROCESS 



################################################################
#### Definición de canales, escuchas y colas de transmisión ####
################################################################

==> Defina el canal emisor en el gestor de colas de origen: 
DEFINE CHANNEL ('source.to.target') +
         CHLTYPE(SDR) +
         CONNAME (RHX5498) +
         XMITQ ('target.queue.manager') +
         TRPTYPE(TCP)

==> Defina el canal receptor en el gestor de colas de origen
DEFINE CHANNEL ('target.to.source') +
         CHLTYPE(RCVR) +
         TRPTYPE(TCP)

==> Defina el escucha en el gestor de colas de origen
DEFINE LISTENER ('source.queue.manager') +
         TRPTYPE(TCP)

==> Defina la cola de transmisión en el gestor de colas de origen: 
DEFINE QLOCAL ('target.queue.manager') +
         USAGE (XMITQ)




==> Defina el canal emisor en el gestor de colas de destino
DEFINE CHANNEL ('target.to.source') +
         CHLTYPE(SDR) +
         CONNAME (RHX7721) +
         XMITQ ('source.queue.manager') +
         TRPTYPE(TCP)

==> Defina el canal receptor en el gestor de colas de destino
DEFINE CHANNEL ('source.to.target') +
         CHLTYPE(RCVR) +
         TRPTYPE(TCP)

==> Defina el escucha en el gestor de colas de destino
DEFINE LISTENER ('target.queue.manager') +
         TRPTYPE(TCP)

==> Defina la cola de transmisión en el gestor de colas de destino
DEFINE QLOCAL ('source.queue.manager') +
         USAGE (XMITQ)



########################################
# Inicio de los escuchas y los canales #
########################################

==> Inicie el escucha en el gestor de colas de origen, source.queue.manager, emitiendo el siguiente mandato MQSC
START LISTENER ('source.queue.manager')

==> Inicie el escucha en el gestor de colas de destino, target.queue.manager, emitiendo el siguiente mandato MQSC
START LISTENER ('target.queue.manager')


==> Inicie el canal emisor en el gestor de colas de origen, source.queue.manager, emitiendo el siguiente mandato MQSC
START CHANNEL ('source.to.target')

==> Inicie el canal emisor en el gestor de colas de destino, target.queue.manager, emitiendo el siguiente mandato MQSC
START CHANNEL ('target.to.source')



########################################
### Definición automática de canales ###
########################################

==> Si WebSphere MQ recibe una petición de conexión de entrada y no puede encontrar un canal receptor 
==> o de conexión de servidor apropiado, crea un canal automáticamente. 
==> Las definiciones automáticas se basan en dos definiciones predeterminadas 
==> suministradas con WebSphere MQ: SYSTEM.AUTO.RECEIVER y SYSTEM.AUTO.SVRCONN.

