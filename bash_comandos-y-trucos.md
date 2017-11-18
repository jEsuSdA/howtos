# Comandos y trucos para BASH (consola)

[Artículo original de linuxito.com](https://www.linuxito.com/gnu-linux/nivel-basico/988-comandos-y-trucos-de-linea-de-comandos-de-viejo-lobo-sysadmin)

## Borrar el contenido de un archivo

Una forma rápida de vaciar un archivo es redirigir una salida nula:

	> archivo

## Volver al directorio anterior

No confundir con ir al directorio padre cd ... Funciona como la flecha izquierda de los navegadores:

	cd -

## Cambiar rápidamente al directorio $HOME

	cd

## Ver un log "en vivo y en directo"

	tail -f /var/log/syslog

## Listar todos los puertos TDP y UDP abiertos

	netstat -tulpn

## Listar todos los sockets Unix abiertos

	netstat -xlpn

## Ejecutar un comando sin que quede registrado en el historial de Bash

Práctico cuando necesitamos incluir una contraseña como parte de un comando, para no divulgarla en el historial. Sólo es necesario dejar un espacio en blanco delante del comando:

	 COMANDO

## Ejecutar un comando sólo si el anterior finaliza correctamente

	apt-get update && apt-get upgrade

## Indicarle a un utilitario que deje de interpretar opciones

Sólo se deben utilizar dos guiones del medio. Por ejemplo quiero buscar dos guiones del medio consecutivos en cierto contenido utilizando grep:

    root@debian:~# history | grep '--'
    Usage: grep [OPTION]... PATTERN [FILE]...
    Try 'grep --help' for more information.

Aunque utilice comillas, grep interpreta el guión del medio como delimitador de una opción. La forma correcta es:

    root@debian:~# history | grep -- --
      167  git commit --amend --reset-author
      508  history | grep '--'

En el primer comando, grep interpreta los guiones como opciones, a pesar de que se utilicen comillas, comillas dobles o escapes. En el segundo ejemplo, primero se indica a grep que deje de interpretar opciones y luego se pasa el parámetro sin inconvenientes.

## Crear varios niveles de directorio en un único comando

Utilizar al opción -p de mkdir:

	mkdir -p /ruta/a/un/directorio/anidado

## Crear y editar un archivo con cat

	cat > archivo

Para finalizar, presionar Ctrl+D (atajo de teclado para EOF, End Of File).

## Listar todos los archivos abiertos

Una forma rápida para saber qué archivos de log están siendo utilizados por demonios o servicios:

    lsof | grep "\.log"

## Omitir a grep en la salida de ps

En el artículo Tip: cómo ocultar el proceso grep en la salida de ps compartí este ingenioso truco.

    root@hal9000:/usr/home/emi # ps x | grep "ntpd"
    1620  -  Ss     0:00.07 /usr/sbin/ntpd -c /etc/ntp.conf -p /var/run/ntpd.pid -f /var/db/ntpd.drift
    1682  2  S+     0:00.00 grep ntpd

Simplemente se debe encerrar cualquier letra del patrón de búsqueda entre corchetes:

    root@hal9000:/usr/home/emi # ps x | grep "[n]tpd"
    1620  -  Ss     0:00.07 /usr/sbin/ntpd -c /etc/ntp.conf -p /var/run/ntpd.pid -f /var/db/ntpd.drift

## Listar los archivos modificados en las últimas 24 horas

Otro interesante comando que compartí previamente en el artículo Listar los archivos modificados en las últimas 24 horas:

    find . -type f -mtime -1 -exec ls -gGh --full-time '{}' \; | cut -d ' ' -f 4,5,7

## Listar sólo las conexiones establecidas

    netstat -tupn | grep EST

## Identificar el proceso asociado a una conexión establecida

Información útil que provee netstat:

    netstat -tupnee

## Obtener estadísticas de red

    netstat -s

## Remover la ruta en un nombre de archivo

El utilitario basename remueve el directorio y sufijo en un nombre de archivo. Muy útil al momento de crear scripts:

    root@linuxito:/usr/local/nginx# pwd
    /usr/local/nginx

    root@linuxito:/usr/local/nginx# basename $(pwd)
    nginx

## Mandar un proceso a segundo plano

Si hemos lanzado un proceso largo en primer plano, y deseamos que continúe ejecutando en segundo plano (como si lo hubiésemos lanzado con &), sólo es necesario presionar Ctrl+Z para suspenderlo y luego bg para que continúe en background.

    emi@hal9000:~ % ping 8.8.8.8 >/dev/null 
    ^Z
    Suspended
    emi@hal9000:~ % jobs
    [1]  + Suspended                     ping 8.8.8.8 > /dev/null
    emi@hal9000:~ % bg 1
    [1]    ping 8.8.8.8 > /dev/null &
    emi@hal9000:~ % jobs
    [1]    Running                       ping 8.8.8.8 > /dev/null

## Retomar un proceso en segundo plano

    fg 1

Donde 1 es el número de tarea en background que nos aparecía entre corchetes al pulsar Ctrl+Z


## Terminar un proceso en segundo plano

Si hemos lanzado un proceso en segundo plano y deseamos terminarlo abruptamente (kill), no es necesario conocer su PID. Es posible matarlo indicando su número de trabajo:

    emi@hal9000:~ % jobs
    [1]    Running                       ping 8.8.8.8 > /dev/null
    emi@hal9000:~ % kill %1
    emi@hal9000:~ % jobs
    [1]    Terminated                    ping 8.8.8.8 > /dev/null

## Cómo saber el pid de la sesión actual

En Bash, la variable $ expande al PID de la sesión actual (instancia de Bash en ejecución):

    echo $$

## Contar filas repetidas

Algo como lo que haría un administrador de bases de datos al usar COUNT(*) en conjunto con GROUP BY. Contar la cantidad de repeticiones de una misma fila utilizando sort y uniq:

    [COMANDO] | sort | uniq -c | sort -nr

## Buscar archivos por contenido

Una vez más grep al rescate, por lejos mi comando favorito:

    grep -r 'ssl' /etc/apache2/*

Este comando busca recursivamente la palabra "ssl" dentro del contenido de todos los archivos dentro del directorio /etc/apache2/.

## Buscar archivos por nombre

Con la opción -iname se ignoran mayúsculas/minúsculas (case) en los nombres de archivo. Utilizar asteriscos para buscar patrones:

    find /etc/apache2/ -iname "*.conf"

## Conocer el formato de un filesystem

Pasar el nombre de dispositivo como parámetro a file:

    root@devuan:~# file -s /dev/sda1 
    /dev/sda1: Linux rev 1.0 ext4 filesystem data, UUID=bed6a1f2-ec59-4913-9af6-0e2f66ac2954, volume name "ROOT" (needs journal recovery) (extents) (large files) (huge files)

## Invertir el orden de las líneas de un archivo

El comando **tac** es cat pero concatena e imprime en orden reverso:

    tac ARCHIVO

    root@devuan:~# cat /etc/shells 
    # /etc/shells: valid login shells
    /bin/sh
    /bin/dash
    /bin/bash
    /bin/rbash
    root@devuan:~# tac /etc/shells
    /bin/rbash
    /bin/bash
    /bin/dash
    /bin/sh
    # /etc/shells: valid login shells

## Invertir un string

El comando rev permite invertir un string (transformar "hola" en "aloh"). Opera sobre cada línea de un archivo de manera individual:

    root@devuan:~# rev /etc/shells
    sllehs nigol dilav :sllehs/cte/ #
    hs/nib/
    hsad/nib/
    hsab/nib/
    hsabr/nib/

## Buscar ayuda

Para buscar ayuda en páginas de manual (por ejemplo, si no conocemos la página de manual que debemos consultar) recurrir a apropos:

    apropos PALABRA_CLAVE

## Imprimir un árbol de procesos

Hay varias formas de hacerlo:

    tree
    ps -ejH
    ps axjf

## Volcar un archivo binario por salida estándar

    hexdump -C ARCHIVO

## Responder "yes" a cualquier comando interactivo

Trivial:

    yes | COMANDO

Yes imprime el caracter 'y' forever, no como el pájaro de Homero.

## Realizar cálculos matemáticos por línea de comandos

La herramienta bc se puede utilizar como calculadora de línea de comandos:

    emi@hal9000:~ % echo "1024*1024" | bc
    1048576

## Obtener rápidamente nuestro id de usuario

Y de todos los grupos a los que pertenecemos:

    emi@hal9000:~ % id
    uid=1001(emi) gid=0(wheel) groups=0(wheel),44(video),920(vboxusers)

## Matar procesos por nombre

Matar o enviar cualquier señal por nombre de ejecutable en vez de PID:

    pkill PROGRAMA

## Conectar con un pipe la stderr

Si necesitamos conectar tanto la salida estandar (stdout) como la salida de errores (stderr) de un proceso, a la entrada estandar (stdin) de otro, debemos agregar ampersand luego del pipe:

    COMANDO1 |& COMANDO2
