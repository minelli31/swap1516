### Práctica 4 - Comprobar el rendimiento de servidores web ###

 1. ApacheBench

      ApacheBench (ab) es un programa de ordenador línea de comandos de un solo subproceso para medir el rendimiento de los servidores web HTTP. [1] Originalmente diseñado para probar el servidor HTTP Apache, que es lo suficientemente genérico como para probar cualquier servidor web.

      Hemos ejecutado un script para la serie de pruebas con la sieguiente instrucción:

      ~~~
        ab -n <nº de peticiones> -c <nº concurrencia> <direccion destinataria>
      ~~~

      ![AB HAPROX](prueba1_haprox.png "ab_haprox")
      ![AB NGINX](prueba1_nginx.png "ab_nginx")
      ![AB M1](prueba1_m1.png "ab_m1")

      Hemos obtenido los siguientes resultados:

      ![Result HAPROX](prueba1_haprox.png "result_haprox")
      ![Result NGINX](ab_nginx.png "result_nginx")
      ![Result M1](ab_m1.png "result_m1")

      La comparativa final de la prueba en ApacheBench:

      ![Result Comparativa](ab_comparativa.png "result_comp") 


 2. Siege

***
