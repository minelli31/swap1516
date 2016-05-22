### Práctica 5. Replicación de bases de datos MySQL ###

1. Crear una BD e insertar datos

  Hemos creado una base de datos en MySQL e insertaremos algunos datos.

  ![Crear 1](tab1.png "crear_1")

2. Replicar una BD MySQL con mysqldump

  ![BLOQUEO BD](block_act.png "bloqueo_bd")

  Creamos una copia local de nuestra base de datos, tenemos que acceder como root:
  ~~~
  > mysqldump contactos -u root -p > /root/contactos.sql
  ~~~
  Comprobamos com * ls -la /root/ * si se ha creado la copia.
  ![LS ROOT](_ls_root.png "ls_root")


3. Replicación de BD mediante una configuración maestro-esclavo


4.
***
