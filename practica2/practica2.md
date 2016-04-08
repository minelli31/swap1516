### Práctica 2 ###

Hemos hecho los siguientes pasos:

 1. Ejecutamos el comando rsync para la transmisión y sincornización eficiente de datos

      * rsync -avz -e ssh swap@192.168.1.101:/var/www/ /var/www/

      ![Máquina 1](var_maq_1.png "Máquina 1")

      ![Máquina 2](var_maq_2.png "Máquina 2")

 2. Hacemos la copia completa del directorio /var/www pero excluyendo /var/www/error , /var/www/stats y /var/www/files/pictures

      * rsync -avz --delete --exclude=**/stats --exclude=**/error --exclude=**/files/pictures -e "ssh -l swap" swap@192.168.1.101:/var/www/ /var/www/

      ![Máquina 2 Exclude](exclude_err_pic.png "Máquina 2 Exclude")
