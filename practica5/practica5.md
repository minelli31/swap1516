### Práctica 5. Replicación de bases de datos MySQL ###

1. Crear una BD e insertar datos

  Hemos creado una base de datos en MySQL e insertaremos algunos datos.

  ![Crear 1](tab1.png "crear_1")

2. Replicar una BD MySQL con mysqldump

  Antes de hacer la copia de seguridad en el archivo .sql debemos evitar que se acceda a la BD para eso bloqueamos la BD.
  ~~~
  mysql> flush tables with read lock;
  ~~~
  ![BLOQUEO BD](block_act.png "bloqueo_bd")

  Creamos una copia local de nuestra base de datos, tenemos que acceder como root:
  ~~~
  #root> mysqldump contactos -u root -p > /root/contactos.sql
  ~~~
  Comprobamos com * ls -la /root/ * si se ha creado la copia.

  ![LS ROOT](_ls_root.png "ls_root")

  Una vez ya hemos creado nuestra copia, procedemos a desbloquear las tablas:
  ~~~
  mysql> unlock tables;
  ~~~
  ![UNLOCK](unlock.png "unlock")

  Enviamos la BD a la otra máquina y la ubicamos en /root/.

  ![ENVIO BD](_envio_bd.png "evio_bd")

  Restauración, en equipo remoto, de la base de datos con la información del equipo maestro y comprobamos la base de datos y consultaremos su contenido, podemos ver que el proceso se ha realizado con éxito.

  ![CREAR BD2](_db2_create.png "crear_bd2")
  ![MOSTRAR DBs 2](_show_db2.png "show_db2")
  
3. Replicación de BD mediante una configuración maestro-esclavo


4.
***
