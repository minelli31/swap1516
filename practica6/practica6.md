### Práctica 6. Discos en RAID ###

1. Configuración del RAID por software

  Primeramente tenemos que configurar un controlador Small Computer System Interface, más conocida por el acrónimo inglés SCSI (interfaz de sistema para pequeñas computadoras), es una interfaz estándar para la transferencia de datos entre distintos dispositivos del bus de la computadora, a través de este controlador SCSI añadiremos 2 discos con la misma capacidad de nuestra máquina principal.

    ![Crear Discos1](img/discos.png "crear_discos")

  Ahora arrancamos la maquina principal y instalamos el software necesario para configurar el RAID

  Usaremos Multiple Device Administrator, MDADM es un conjunto de herramientas que son utilizadas en GNU/Linux para la gestión de RAID (Redundant Array of Independent Disks, que se traduce como conjunto redundante de discos independientes) administrado a través de software, distribuido bajo los términos de la Licencia Pública General de GNU versión 2 (GNU/GPLv2).

  El RAID por software permite:

    * Una solución de RAID de muy bajo costo pues prescinde del uso de costosas tarjetas RAID y unidades de almacenamiento especiales y que carece de restricciones que regularmente imponen los fabricantes de dispositivos de hardware privativos.

    * Proceso de reconstrucción de arreglos en subprocesos.

    * Configuración basada sobre el núcleo del sistema.

    * Permite portar de manera transparente los arreglos entre sistemas GNU/Linux sin necesidad de reconstruir éstos.

    * Mejor aprovechamiento de los recursos del sistema pues la reconstrucción de los arreglos se hace utilizando recursos libres.

    * Soporte para unidades de almacenamiento con capacidad para realizar cambios en caliente (hot-swap).

    * Detección automática del número de núcleos del microprocesador a fin de aprovechar mejor los recursos del sistema.

    Después de instalar el MDADM ejecutamos *sudo fdisk -l*

  **Durante el proceso de insalacion nos preguntará si queremos intalar Postfix, elegimos la opcion Sin configuración**

  ![FDISK -L](img/fdisk.png "fdisk_l")

  Ahora tenemos 3 discos, en los cuales el primero es en el que esta instalado el sistema, por eso nos muestra su tabla de particiones los otros 2 son los que hemos creado a través del controlador SCSI y no tienen tabla de particiones, todavia entán vacios.

  Ahora ya podemos crear el RAID 1. Para ello "crearemos" un dispositivo nuevo, /dev/md0, que "contendra" los dos discos que hemos creado, /dev/sdb y /deb/sdc

  ![FDISK -L](img/raid_1.png "raid 1")

  **El dispositivo se creó con el nombre /dev/md0**

  Después de crear el dispositivo RAID inicializamos un dispositivo de almacenamiento con *mkfs* y creamos el directorio en el que se montará la unidad del RAID con *mkdir* y lo montamos con *mount*:

  ~~~
  #root> mkfs /dev/md0
  #root> mkdir /dat
  #root> mount /dev/md0 /dat
  ~~~

  Comprobamos si se ha montado correctamente con *mount* y el estado del RAID con *mdadm --detail /dev/md0*:

  ~~~
  #root> mount
  #root> mdadm --detail /dev/md0
  ~~~
  ![FDISK -L](img/mount.png "mount")
  ![FDISK -L](img/detail.png "detail")

  Procedemos a editar el fichero */etc/fstab* y agregar el montaje automático del dispositivo que hemos creado y para esto necesitamos su UUID, con el comando *ls -l /dev/disk/by-uuid/*

  ![FDISK -L](img/uuid.png "uuid")
  ![FDISK -L](img/fstab.png "fstab")

2. Comprobación de los RAID

  Ahora simularemos un fallo en uno de los discos y comprobaremos que el dispositivo RAID funciona correctamente:

  ~~~
  #root> mdadm --manage --set-faulty /dev/md0 /dev/sd
  ~~~

  ![FDISK -L](img/prueba_1.png "prueba 1")

  Comprobamos el estado del RAID, observamos que la unidad /dev/sdb esta en estado de fallo.

  Ahora procedemos a retirar *en caliente* el disco que está marcado como que ha fallado:

  ~~~
  #root> mdadm --manage --remove /dev/md0 /dev/sdb  
  ~~~

  ![FDISK -L](img/hot_remove.png "remover disco en caliente")

  Comprobamos el estado del RAID y podemos ver como la unidad /dev/sdb no aparece.

  Añadimos de la misma manera *en caliente* un nuevo disco que reempazará al disco que hemos retirado:

  ~~~
  #root> mdadm --manage --add /dev/md0 /dev/sdb
  ~~~

  ![FDISK -L](img/hot_add.png "añadir disco en caliente")

  Comprobamos que la unidad /dev/sdb está activa en nuestro sistema.

3. Configuración del servidor NFS

  Instalaremos e configuraremos el servidor NFS, para poder disfrutar del servicio de compartir carpetas en la red mediante NFS, en el PC servidor es necesario instalar el paquete del servidor NFS. Lo normal es que todos los PCs dispongan del paquetes servidor de NFS ya que en cualquier momento puede existir la necesidad de tener que compartir una carpeta desde cualquier PC, aunque lo habitual es que el único que comparta sea el servidor. Que un PC de un usuario tenga instalado el paquete del servidor NFS, no significa que automáticamente esté compartiendo su sistema de archivos en la red. Para ello es necesario configurar y arrancar el servicio.

  ~~~
  #root> apt-get install nfs-common nfs-kernel-server
  ~~~

  ![FDISK -L](img/install_nfs.png "install nfs")

  Error al tratar de arrancar el servicio antes de configurarlo.

  ![FDISK -L](img/error_start.png "error start")

  Antes de arrancar el servicio NFS, es necesario indicar qué carpetas deseamos compartir y si queremos que los usuarios accedan con permisos de solo lectura o de lectura y escritura. También existe la posibilidad de establecer desde qué PCs es posible conectarse. Estas opciones se configuran en el archivo */etc/exports*

  En cada línea del archivo de configuración del servidor NFS /etc/exports, se puede especificar:

    * La carpeta que se quiere compartir
    * El modo en que se comparte (solo lectura 'ro' o lectura y escritura 'rw' )
    * Desde qué PC o PCs se permite el acceso (nombre o IP del PC o rango de IPs)

  ![FDISK -L](img/exports.png "exports")

  Iniciamos el servicio

  ~~~
  #root> service nfs-kernel-server start
  ~~~
  ![FDISK -L](img/start_service.png "start service nfs")

  Ahora configuraremos la máquina 2.

  Necesitaremso montar el servicio en nustra máquina 2 para incorporar un espacio compartido *nfs*, para eso haremos uso de la orden *mount* y editaremos el fichero */etc/fstab*.

  ~~~
  #root> mount -t nfs -o rw,nosuid 192.168.1.100:/dat /home/swap2/test
  ~~~

  ![FDISK -L](img/mount_swap2.png "mount swap2")
  ![FDISK -L](img/fstab_swap2.png "fstab swap2")

  Comprobamos que todo funciona añadiendo y quitando ficheros en la máquina uno, todo el proceso se hace paralelamente en la máquina 2 como un "espejo" de la máquina 1.

  ![FDISK -L](img/test_final.png "test final")

***
