1.ENTORNO DE DESARROLLO SYNFONY 3
=================================

1.1.Instalar Composer
----------------------

Composer es un manejador de dependencias. Composer es un manejador de dependencias, no un gestor de paquetes. Pero es cierto que trata con paquetes y librerías, la instalación siempre es local para cualquier proyecto, las librerías se instalan en un directorio por defecto (normalmente es /vendor).
* Entramos dentro de dominio de [Composer](https://getcomposer.org/download/) y buscamos el link de descarga de su [setup](https://getcomposer.org/Composer-Setup.exe).
* Durante la instalación habrá que indicar la ubicación del ejecutable **PHP** del servidor local. En este caso al usar **Wamp64** es: ```C://wamp64/bin/php/php7.0.10/php.exe```.
* Para comprobar que se instaló correctamente hay que introducir dentro de la terminal el comando ```composer -v```, este nos devolverá información sobre la versión instalada de **Composer**.

1.2.Instalar CYGWIN
-------------------

Cygwin es una colección de herramientas desarrollada por Cygnus Solutions para proporcionar un comportamiento similar a los sistemas Unix en Microsoft Windows. Su objetivo es portar software que ejecuta en sistemas POSIX a Windows con una recompilación a partir de sus fuentes.
*Nota: terminal tiene toda la potencia de la terminal de Linux pero simulada en Windows.*

* Entramos en el dominio de [CygWin](https://www.cygwin.com/) y buscamos el link de descarga de su [setup](https://cygwin.com/setup-x86_64.exe).
* Se instalará la consola dentro de la carpeta que propone por defecto ```C://CygWin64/```. En cambio si habrá que indicar dónde se instalaran los **Package**, se recomienda dentro de: ```C://Cygwin_Packages/```.
* Acontinuación se indicarán los paquetes que más podrían interesarnos instalar (todos instalados usando Bin):
  * *GIT -> Devel Default -> git: Distributed version control system*.
  * *SSH -> Net Default -> opnssh: The Open SSH server and client programs*. 
  * *NANO -> Editors Default -> nano: Enhanced clone of Pico editor*.
  * *WGET -> Web Default -> wget: Utility to retrieve files from the WWW via HTTP and FTP*.
Para comprobar el correcto funcionamiento ejecutamos **Cygwin64**, y dentro de la consola escribimos ```git -v```, ```git --version```, ```ssh -v```, ```nano -v```,...

1.3.Instalar NetBeans
---------------------
NetBeans es un entorno de desarrollo integrado libre, hecho principalmente para el lenguaje de programación Java. Existe además un número importante de módulos para extenderlo. NetBeans IDE es un producto libre y gratuito sin restricciones de uso.

1.4.Instalar Symfony3
---------------------
* Ejecutamos la consola de **CygWin**.
* Accederemos a la carpeta del servidor local escribiendo: ```cd c:/wamp64/www```.
* Una vez dentro de la carpeta dónde queremos iniciar el proyecto (se encontrará dentro del servidor web **Wamp**) ejecutamos: ```composer create-project symfony/framework-standard-edition symfony/ "3.0.0"```. 

*Nota<sup>1</sup>: La primera ubicación indica el directorio desde dónde se recogen los archivos de symfony, es decir dónde se instaló inicialmente. En cambio la segunda ubicación definirá dentro del directorio de ejecución la carpeta dónde se ubicará el proyecto. La parte dónde se escribe ```"3.0.0"``` especificará la versión de symfony que desea instalarse.*

* Después mostrará un asistente de configuración dónde se indicará:
```
database_host (127.0.0.1):    /     /dirección ip del servidor
database_port (null):               // puerto
database_name (symfony): pruebas    //nombre de la base de datos
database_user (root):
database_password (null):
...
```

*Nota<sup>2</sup>: si entramos en la siguiente url ```http://localhost/symfony/web/config.php``` se comprobará la configuración de la instalación. Si por alguna razón hubiera problemas es en esta dirección dónde se mostrará dicho error.

*Nota<sup>3</sup>* **Modificar php.ini**: *Dentro del archivo: ```C:\wamp64\bin\apache\apache2.4.23\bin\php.ini``` colocar debajo de ```intl.error_level = E_WARNING``` la línea ```intl.error_level = 0```. Y colocar debajo de la directiva ```xdebug.show_local_vars=0``` esta otra ```xdebug.max_nesting_level=250```*.

Una vez hechos los cambios anteriores, podemos acceder a la siguiente dirección: ```http://localhost/symfony/web/```

1.5.Crear hosts virtuales en Apache
-----------------------------------
IMPORTANTE: SIEMPRE TENER EL MODULO REWRITE DE APACHE ACTIVADO

**AVISO: Esto es opcional, no es necesario que lo hagas para seguir el curso **

Cuando creamos un virtualhost lo que estamos haciendo es simular en nuestro servidor apache local un dominio real, de forma que en lugar de entrar a [http://localhost/symfony3/web](http://localhost/symfony3/web) entraríamos a [http://symfony3.com.devel](http://symfony3.com.devel).

Vamos a ver como se crean los virtualhost en WampServer, aunque esto es muy similar en cualquier versión de Apache.

**Paso 1.** Entrar al fichero ```C:\wamp\bin\apache\apache2.4.9\conf\httpd.conf``` y añadir o descomentar el include del fichero de los hosts virutales:

Include conf/extra/httpd-vhosts.conf
**Paso 2.** Entrar al fichero ```C:\wamp64\bin\apache\apache2.4.9\conf\extra\httpd-vhosts.conf``` y añadir los virtualhosts, en mi caso voy a crear 3 nuevos virtualhosts, uno para localhost, otro para un proyecto de Zend Framework 2 y otro para un proyecto de Symfony 3.
```php
# LOCALHOST
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot "c:/wamp/www"
    ServerName localhost
    ErrorLog "logs/localhost-error.log"
    CustomLog "logs/localhost-access.log" common
</VirtualHost>
# VHOST CURSO ZF2
<VirtualHost *:80>    
    DocumentRoot "c:/wamp/www/zend2/public"
    ServerName zend2.com.devel
    ServerAlias www.zend2.com.devel
    <Directory "c:/wamp/www/zend2/public">
        Options Indexes FollowSymLinks        
        AllowOverride All
        Order Deny,Allow
        Allow from all        
    </Directory>    
</VirtualHost>
# VHOST CURSO SYMFONY 3
<VirtualHost *:80>    
    DocumentRoot "c:/wamp/www/symfony3/web
    ServerName symfony3.com.devel
    ServerAlias www.symfony3.com.devel
    <Directory "c:/wamp/www/symfony3/web">
        Options Indexes FollowSymLinks        
        AllowOverride All
        Order Deny,Allow
        Allow from all        
    </Directory>    
</VirtualHost>
```

**Paso 3.** Añadir al fichero hosts de nuestro sistema, en el caso de Windows ```C:\Windows\System32\drivers\etc\hosts``` (si estas en Windows 8 o 10 ejecuta el programa de edición de código como Administrador para poder guardar los cambios), y añadir las IP y las url.
```
127.0.0.1       localhost
127.0.0.1    zend2.com.devel
127.0.0.1    symfony3.com.devel
Lo que le estamos indicando es que cuando carguemos cualquiera de esos dominios nos llame a la IP que le indicamos en este caso 127.0.0.1 en lugar de la IP original del dominio si es que la tiene.
```
Ahora si entramos a [http://symfony3.com.devel](http://symfony3.com.devel) nos abrirá la web que tenemos en nuestro directorio www.