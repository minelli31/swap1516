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

  Lo primero que haremos, sera configurar el MySQL del equipo maestro. Para ello editaremos como root el fichero /etc/mysql/my.cnf:

    * Comentamos el parámetro bind-address. Este sirve para que escuche a un servidor.
    * Le indicamos el archivo donde almacenar el log de errores. De esta forma, si por ejemplo al reiniciar el servicio cometemos algún error en el archivo de configuración, en el archivo de log nos mostrará con detalle lo sucedido.
    * Establecemos el identificador del servidores.
    * Le indicamos el archivo de registro binario. El registro binario contiene toda la información que está disponible en el registro de actualizaciones, en un formato más eficiente y de una manera que es segura para las transacciones.

  Guardamos las configuraciones y reiniciamos el servicio, con unos de los comandos a seguir:
    ~~~
    #root> service mysql restart
    #root> /etc/init.d/mysql restart
    ~~~
    ![MY CNF 1](_my_cnf_1.png "my_cnf_1")
    ![MY CNF 2](_my_cnf_2.png "my_cnf_2")

  Si no nos ha dado ningún error la configuración del maestro, podemos pasar a hacer la configuración del mysql del esclavo. Para ello editaremos como root el fichero /etc/mysql/my.conf

  La configuración es similar a la del maestro, con la diferencia del Paso 2 (server-id) que en este caso es 2.

  En el maestro creamos un usuario y le damos permis de acceso a la replicación, con los siguientes comandos.

    ![CREATE 2](create2.png "create_2")
    ![STATUS](sec_status.png "status")

  En el esclavo introducimos el siguiente comando: (Tenemos que parar el esclavo para la ejecución de la sentencia, luego volvemos a iniciarlo)

  ~~~
  mysql> CHANGE MASTER TO MASTER_HOST='192.168.1.100',
  MASTER_USER='esclavo', MASTER_PASSWORD='esclavo',
  MASTER_LOG_FILE='mysql-bin.000017', MASTER_LOG_POS=841,
  MASTER_PORT=3306;
  ~~~
  ![CHANGE MASTER](sec_chg_master.png "change_master")

En el equipo maestro activamos las tablas con:
~~~
mysql> mysql tables;
~~~
Ejecutamos el siguiente comando en el equipo esclavo para verificar si todo esta correcto:
~~~
mysql> SHOW SLAVE STATUS\G
~~~
Si el valor de Second_Behind_Master es distinto de null, todo estará correcto.

  ![STATUS G](status_g.png "status_g")

 Comprobamos que todo esta bien insertando en la base de datos del maestro un nuevo dato, si esta todo correcto este dato se insertará automáticamente en la base de datos del equipo esclavo.

 ![PRUEBA MAESTRO](prueba_sw1.png "prueba maestro")
 ![PRUEBA ESCLAVO](prueba_sw2.png "prueba esclavo")





***
