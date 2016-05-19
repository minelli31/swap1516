### Práctica 4 - Comprobar el rendimiento de servidores web ###

1. ApacheBench


      ApacheBench (ab) es un programa de ordenador línea de comandos de un solo subproceso para medir el rendimiento de los servidores web HTTP. [1] Originalmente diseñado para probar el servidor HTTP Apache, que es lo suficientemente genérico como para probar cualquier servidor web.

      Hemos ejecutado un script para la serie de pruebas con la sieguiente instrucción:


        > ab -n <nº de peticiones> -c <nº concurrencia> <direccion destinataria>

     * nº de peticiones: es el nº total de peticiones que se enviara al servicor destinatario

     * nº de concurrencia: cantidad de peticiones que se enviaran al ervidor destinatario, en cada solicitud

     * direccion destinataria: direccion o identificador del servidor al que enviaremos las peticiones

      ![AB HAPROX](prueba1_haprox.png "ab_haprox")
      ![AB NGINX](prueba1_nginx.png "ab_nginx")
      ![AB M1](prueba1_m1.png "ab_m1")

      Hemos obtenido los siguientes resultados:

      ![Result AB HAPROX](ab_haprox.png "result_haprox_ab")
      ![Result AB NGINX](ab_nginx.png "result_nginx_ab")
      ![Result AB M1](ab_m1.png "result_m1_ab")

      La comparativa final de la prueba en ApacheBench:

      ![Result Comparativa](ab_comparativa.png "result_comp")

      Podemos ver en la comparativa final que tarda más el servidor que tiene el balanceador de carga, esto se debe, a que el "camino" se se recorre a traves del balanceador de carga es mas largo. Tambien podemos ver como nginx tarda menos que haproxy.

      Además podemos ver como el servidor final responde mas peticiones por segundo que el balanceador de carga. Algo normal, debido a que este es mas rapido como hemos visto en la grafica anterior.

2. Siege

      El Siege fue diseñado para permitir a los desarrolladores web mediren el rendimiento de su código bajo estrés, para ver cómo se comporta el código frente a la carga en Internet.

      > siege <opciones de ejecucion> <direccion destinataria>


    Las opciones de ejecucion que utilizaremos seran -b -t60S -v
      * b ejecutaremos los tests sin pausas con lo que comprobaremos el rendimiento general
      * t60s ejecutaremos siege durante 60 segundos
      * v le indicamos que nos muestre mas informacion
    Direccion o identificador del servidor al que enviaremos las peticiones

    ![SIEGE HAPROX](prueba1_siege_haprox.png "siege_haprox")
    ![SIEGE NGINX](prueba1_siege_nginx.png "siege_nginx")
    ![SIEGE M1](prueba1_siege_m1.png "siege_m1")

    Hemos obtenido los siguientes resultados:

    ![Result SIEGE HAPROX](siege_haprox.png "result_siege_haprox")
    ![Result SIEGE NGINX](siege_nginx.png "result_siege_nginx")
    ![Result SIEGE M1](siege_m1.png "result_siege_m1")

    La comparativa final de la prueba en ApacheBench:

    ![Result SIEGE Comparativa](siege_comparativa.png "result_comp_siege")
***
