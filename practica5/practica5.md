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
    sudo service mysql restart
    /etc/init.d/mysql restart
    ~~~
    ![MY CNF 1](_my_cnf_1.png "my_cnf_1")
    ![MY CNF 2](_my_cnf_2.png "my_cnf_2")
4.
***
