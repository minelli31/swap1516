### Práctica 2 ###

Hemos hecho los siguientes pasos:

 1. Ejecutamos el comando rsync para la transmisión y sincornización eficiente de datos

      * rsync -avz -e ssh swap@192.168.1.101:/var/www/ /var/www/

      ![Máquina 1](var_maq_1.png "Máquina 1")

      ![Máquina 2](var_maq_2.png "Máquina 2")


 2. Hacemos la copia completa del directorio /var/www pero excluyendo /var/www/error , /var/www/stats y /var/www/files/pictures

      * rsync -avz --delete --exclude=**/stats --exclude=**/error --exclude=**/files/pictures -e "ssh -l swap" swap@192.168.1.101:/var/www/ /var/www/

      ![Máquina 2 Exclude](exclude_err_pic.png "Máquina 2 Exclude")


 3. Acceso sin contraseña para ssh

      * ssh-keygen -t dsa

      ![ssh-keygen](ssh-keygen.png "ssh-keygen")


4. Usamos  el comando "ssh-copy-id" para hacer una copia de la clave a la máquina principal (a la que querremos acceder luego desde la secundaria):

     * ssh-copy-id -i .ssh/id_dsa.pub  swap@192.168.1.101
     ![ssh-copy-id](ssh-copy-id.png "ssh-copy-id")

     Conectarnos a dicho equipo sin contraseña:

     * ssh 192.168.1.101 -l swap
     ![SSH Máquina 1](ssh-maquina_1.png "SSH Máquina 1")

     Para ejecutar comandos añadimos al final del comando ssh para conectarnos:

     * ssh 192.168.1.101 -l swap uname -a
     ![SSH Máquina Uname 1](ssh-maquina_uname.png "SSH Máquina Uname 1")



 5. Programar tareas con crontab

      * /etc/contrab
      ![crontab](crontab.png "crontab")

***
