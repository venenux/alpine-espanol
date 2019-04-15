# HTTP LAMP server recetario

Hoy en dia todo es web, y hay dos tipos, la web 1.0 que es contenido estatico 
y 2.0 que son las webs dinamicas donde no es el webmaster el que fabrica el contenido.

Sin embargo ambas sea 2.0 dinamica o 1.0 necesitan siempre un servidor web estatico. 
y en este documento configuraremos el populacho LAMP stack conocido como la 
formulita de "apache2 + MySQL + PHP en Linux", cambiando mysql por mariadb (hoy estandar)
ya que casi ninguna distro emplea ya mysql.

## Mapa de entorno profesional

Un verdadero servidor web se configura con un DNS que reparte la carga, pero aqui
lo unico que no **tendremos es una configuracion basica para iniciados**

Este documento es para iniciados, basico, si desea algo serio, debe emplear estas diferencias:

* Cambiar MySQL o MariaDB por Percona Server, percona es un mysql superior.
* Definir un http webserver que reciba las peticiones http y reparta a sus apache
* Definir un DNS que reciba las peticiones y las envie al http webserver anterior.

> "El problema de los proyecticos linux es que no salen de un routercito"

## Instalacion de Apache

#### Apache2

El apache2 en alpine esta mas diseccionado que en otras distros.

```
apk update
apk install apache2 apr-util apache2-utils curl

rc-update add apache2
rc-service apache2 start

echo "works" > /var/www/localhost/htdocs/index.html

curl http://127.0.0.1 | grep orks
```

Veras en la consola la palara "works" despues del ultimo comando si todo marcha bien.

#### Lista de paquetes apache2

Por orden de necesidad, separados por utilidad y prioridad sobre otros:

* apache2 : si solo instala este solo los HTML seran servidos, comunmente puerto 80
* apache2-doc : si no instala este no hay manpage, recomendado iniciados y novatos
* apache2-utils : para tener comandos `apachectl` o `htpasswd`, rotar logs con `rotatelogs`, etc
* apr-util : base de utilidades para libreria de acceso de apache
* apache-mod-fcgid : soporte de fast cgi, el mismo protocolo CGI pero mejorado
* apache2-http2 : soporte para el nuevo formato de peticon HTTP 2 en al internet
* apache2-proxy : hace de puerta de enlace para redireccionar peticiones desde/hacia otros servidores
* apache2-proxy-html : no entendi para que separar, pero el mismo que antes pero solo para http/https
* apache2-ssl : para que tenga encriptacion por https aparte de http, comunmente puerto 443
* apache2-webdav : acceso sistema de ficheros pero usando http en vez de red o bus, con autenticacion.
* apache2-lua : para servir web dinamicas empleando leguaje lua
* php5-apache2 : para servir web dinamicas empleando el famosisimo php pero 5.6.X
* php7-apache2 : para servir web dinamicas pero empleando las nuevas php 7.X (a la fuerza pro php devels)
* apache2-mod-wsgi : procesar scripts Python (usando wsgi), es ecir reidirigiendo la salida al webserver
* apache-mod-auth-kerb : autenticacion empleando kerberos a las paginas de apache
* apache-mod-auth-ntlm-winbind : autenticacion pero con el usuario de samba/windo a las paginas de apache
* apache-mod-auth-radius : autenticacion a los modulos d monitoreo y control radius pero con apache
* apache2-ldap : authenticacion de acceso al webserver pero usando ldap (el malvado directorio activo)
* apache2-icons : los estupidos iconos al listar directorios y otras cosas en apache2
* apache2-error : provee en cada error de http una pagina ajustada a ello incluso en multi idiomas
* apache2-dev : necesario si tiene programas a compilar que configuren un mod apache, ejemplo mod-fcgid

## php y apache

Una vez instalado tenemos apache ejecutando, entonces le adicionamos soporte php, 
en alpine tenemos dos ramas de php entre las cuales tenemos 5 y 7 en las versiones 
siguentes php 5.6, php 7.1, php 7.2 y solo en las ultimas php 7.3 no muy compatible.

#### php 5.6

Este es el mas compatible con todo software y la mas efectiva, 
**OJO: php5 5.6 esta en el repositorio main para todos los alpines**

```
apk install php5-apache2 php5 php5-cli php5-fpm php5-cgi php5-sockets php5-phar php5-iconv php5-curl php5-pear

echo "<?php echo phpinfo();?>" > /var/www/localhost/htdocs/index.php

rc-service apache2 restart
curl http://127.0.0.1/index.php | grep engine
```

Veras en la consola la palara "Zend engine" despues del ultimo comando si todo marcha bien.

***Pero, pero pero, tranca palanca** esta es una manera muy bien integrada 
pero solo optimizada para paginas pequeñas, para servicios de alto volumen pero archivos pequeños 
se recomienda el `php-fpm` (en este caso `php5-fpm` junto con `apache2-proxy`), 
ojo para paginas sean grandes o pequeñas pero archivos grandes no usar `php-fpm`!

```
apk add apache2-proxy php5-fpm
```

Despues hay que comentar el archivo `httpd.conf` en la seccion de server agregando:

```
ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/var/www/localhost/htdocs/$1
DirectoryIndex index.php
```

Ahora reiniciar los nuevos servicios:

```
rc-service apache2 restart
rc-update add php5-fpm
```

#### php 7

Esta es la que los de php nos estan obligando/forzando usar sin necesidad alguna:
**OJO: php7 7.1/7.2 esta en el repositorio contriution solamente**

```
apk install php7-apache2 php7 php7-cli php7-fpm php7-cgi php7-sockets php7-phar php7-iconv php7-curl php7-pear

echo "<?php echo phpinfo();?>" > /var/www/localhost/htdocs/index.php

curl http://127.0.0.1/index.php | grep engine
```

Veras en la consola la palara "Zend engine" despues del ultimo comando si todo marcha bien.

#### Lista de paquetes php

Por orden de necesidad, separados por utilidad y prioridad sobre otros:

* php5-common
* php5-apache2
* php5-event
* php5-pear
* php5-curl
* php5-phar
* php5-bcmath
* php5-bz2
* php5-cgi
* php5-fpm
* php5-ftp
* php5-gd
* php5-iconv
* php5-json
* php5-gettext
* php5-xml
* php5-xmlreader
* php5-xmlrpc
* php5-xmlwriter
* php5-xsl
* php5-zip
* php5-openssl
* php5-dba
* php5-session
* php5-sockets
* php5-sqlite3
* php5-ctype
* php5-doc
* php5-dom
* php5-gmp
* php5-imap
* php5-intl
* php5-ldap
* php5-fileinfo
* php5-litespeed
* php5-mbstring
* php5-mysqli
* php5-mysqlnd
* php5-odbc
* php5-pgsql
* php5-pcntl
* php5-pdo
* php5-pdo_dblib
* php5-pdo_mysql
* php5-pdo_odbc
* php5-pdo_pgsql
* php5-pdo_sqlite
* php5-embed
* php5-pspell
* php5-recode
* php5-enchant
* php5-exif
* php5-calendar
* php5-posix
* php5-soap
* php5-shmop
* php5-simplexml
* php5-snmp
* php5-sysvmsg
* php5-sysvsem
* php5-sysvshm
* php5-tidy
* php5-tokenizer
* php5-opcache
* php5-sodium
* php5-wddx
* php5-phpdbg
* php5-dev

## Lua y apache

(WIP)

## MySQL MariaDB

Hoy dia MySQL no es distribuido en linux, se emplea es MariaDB, no preocuparse, 

(WIP)


