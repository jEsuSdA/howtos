
# HOWTO GIT

Muy resumido, claro...

---

Lo más usado:

Habitualmente, comenzaremos cada sesión con:

	git pull origin master

Para descargar los cambios actuales del repositorio

Luego operaremos con nuestro proyecto y, cada cierto tiempo iremos haciendo para aplicar los cambios hechos

	git add -A
	git commit -m "mensaje"

Y para subir al repositorio compartido esos cambios (y terminar):

	git push origin master



## Configuración inicial de Git

Crear usuario

	git config --global user.name "USUARIO"

Consultar

	git config --global user.name


Configuramos el correo

	git config --global user.mail "MI@CORREO.COM"

Activamos el coloreado de sintaxis de git.

	git config --global color.ui true

muestra listado de opciones de configuración de git

	git config --global --list


## Repositorio local de Git

Inicio del proyecto. Sólo lo usamos una vez.

	git init

Consultar estado del proyecto

	git status


Agregar un archivo al proyecto

	git add <archivo>


Agregar todos los archivos al proyecto

	git add -A

Creación de un commit

	git commit -m "Mensaje"

como un git add -A + git commit -m "Mensaje"

	git commit -a -m "Mensaje"


resumen de los cambios hechos en el proyecto (commits y sus códigos identificativos).

	git log


podemos viajar en el tiempo entre commits o ramas.

	git checkout <commit code>

viaja al último commit:

	git checkout master



---
salta hacia atrás, pero deshaciendo los commit que quedan delante del punto al que viajamos.

	git reset

salta atrás, pero NO modifica nuestro código (working area).

	git reset --soft

salta atrás, pero NO modifica el working area ni el stagin area (NO SE USA).

	git reset --mixed

salta atrás, DESCARTANDO los commit y el código descartados.

	git reset --hard


# GITHUB

Github es un servicio que permite tener repositorios de proyectos compartidos de forma gratuita... siempre y cuando sean proyectos públicos y de software libre. Se pueden tenre proyectos privados, pero pagando.

Cómo operar:

0 - Si hay otras personas operando con el proyecto, primero has de descargar los cambios que ellos hayan hecho en el proyecto:
	
    git pull origin master

1 - modifica tus archivos, añade, borra,... trabaja en tu proyecto.

2 - Esto aplica los cambios

	git add -A

3 - Esto integra los cambios en git local

	git commit -m "mensaje"


4 - Esto lo sube a github

	git push origin master




## Clonar un repositorio ya existente

También puede ser que tú te quieras agregar a un repositorio ya existente para colaborar, así que lo primero que tienes que hacer es clonarlo en local:

	git clone https://github.com/proyecto.git


Posteriormente tendrás que hacer una actualización de tu repositorio local desde el de github:

	git pull origin master


Para cuando hay conflicto porque olvidamos hacer pull antes de commit y push.

	git rest --hard





# GIT-FTP

Git-ftp es una utilidad (un script de bash) que permite montar un repositorio remoto vía ftp.

Es decir, que tenemos nuestro repositorio local, y cuando hacemos cambios en él, git-ftp se encarga de aplicar esos cambios al repositorio remoto vía ftp.

La ventaja es que sólo aplica los cambios del último commit. Es decir, que no tenemos que resubir o modificar el ftp manualmente, ya que git-ftp lo hace todo mucho más inteligente y rápidamente.

[https://github.com/git-ftp/git-ftp](https://github.com/git-ftp/git-ftp)





## CONFIGURAR GIT-FTP

Copiar el archivo git-ftp a una ruta en el path.

Para hacer el despliegue, utilizo git-ftp.
Lo configuro de la siguiente forma:

    git config git-ftp.url "ftp://ftp.RUTAFTP"
    git config git-ftp.user "USUARIO-FTP"
    git config git-ftp.password "CONTRASEÑA-FTP"


Ahora iniciamos el push ftp

    git-ftp init

y tras cada commit haremos

    git-ftp push



## GIT-FTP-IGNORE

Igual que con **.gitignore**, se pueden ignorar archivos y carpetas con git-ftp. Sólo hay que crear un archivo llamado **.git-ftp-ignore** en la raíz del proyecto (en el mismo nivel que la carpeta .git, no dentro de .git), editarlo y añadir las expresiones regulares pertinentes.

Pej:


    # Ignoramos los archivos BD para evitar que se sobreescriba la Base de datos
    # de forma accidental
    # En git-ftp se usa una nomenclatura distinta a .gitignore.
    
    \.db$
    # \.db$ == *.db


    # La siguiente expresión regular sirve para ignorar el archivo .htaccess de la carpeta html/blog:
    # ^. html/blog/\.htaccess$   == /html/blog/.htacess
    







# HOOKS


Los hooks son scripts que lanzamos tras hacer una acción de git.

Pej. ejecutar "algo" tras hacer un git commit.

Esto nos puede servir para, por ejemplo, ejecutar un git-ftp push cada vez que hagamos un commit en local.

Para ello, creamos el archivo post-commit dentro de la carpeta de nuestro proyecto

	.git/hooks/

Le damos permisos de ejecución (chmod +x)
Y tenemos que añadir el 

	#!/bin/bash 

al inicio del archivo.

A continuación, escribimos el script que necesitemos lanzar tras cada commit (pej. hacer un deployment).

Si sumamos HOOKS + GIT-FTP, podemos conseguir que, cada vez que hagamos un commit, se actualice el servidor FTP con los cambios realizados.

Para ello, creamos un archivo post-commit en .git/hooks/ y ponemos:


    #!/bin/bash

    # Usamos GIT-FTP para subir los cambios al servidor web vía FTP.

    echo "-----------------------"
    echo "git-ftp push ..."
    ../git-ftp push
    echo "-----------------------"


Así, cada vez que hagamos un commit, la web se actualizará. ;)
