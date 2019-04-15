# HTTP web server avanzado

Hoy en dia todo es web, y hay dos tipos, la web 1.0 que es contenido estatico 
y 2.0 que son las webs dinamicas donde no es el webmaster el que fabrica el contenido.

Sin embargo ambas sea 2.0 dinamica o 1.0 necesitan siempre un servidor web estatico. 
y en este documento configuraremos el mas poderoso (empleado por wikipedia y google) 
nada mas y nada menos que `lighttpd`, como punto de entrada para todos su servicios.

## Mapa de entorno profesional

> "El problema de los proyecticos linus es que no salen de un routercito"

Un verdadero servidor web se configura con un DNS que reparte la carga, pero aqui
lo unico que no tendremos es el DNS, en su lugar estara el poderoso `lighttpd` y 
la "reparticion de bienes" la realizara el increible modulo "proxy" empleando un reverse proxy.

Este documento es para nivel medio a avanzados, se recomienda lo siguiente:

* Cambiar MySQL o MariaDB por Percona Server, percona es un mysql superior.
* Definir un DNS que reciba las peticiones y las envie al http webserver anterior.

> "El problema de los proyecticos linux es que no salen de un routercito"

## Instalacion de webserver

#### Lighttpd

El lighttpd en alpine no esta mjy diseccionado, son 3 paquetes primordiales

```
apk update
apk install 

rc-update add lighttpd default
rc-service lighttpd start

echo "works" > /var/www/localhost/htdocs/index.html

curl http://127.0.0.1 | grep orks
```

Veras en la consola la palara "works" despues del ultimo comando si todo marcha bien.

#### Lista de paquetes lighttpd

Por orden de necesidad, separados por utilidad y prioridad sobre otros:

* lighttpd : paquete base trae casi todo practicamente incluyendo cgi, fcgi, http2, https entre otros.
* lighttpd-doc : si no instalas este, no tendras manpages!!! (ilogica la forma de empaquetar en alpine)
* lighttpd-mod_auth : modulo de autenticacion sea por htpasswd o por mysql
* lighttpd-mod_webdav : modulo de acceso de filesystem por http/https
* acf-lighttpd : interfaz de administracion del lighttpd
* lighttpd-dbg : binario de depuracion para encontrar bugs

Modulo perdido auth-mysql (o me extraña en alpine hay muchas cosa que faltan) https://bugs.alpinelinux.org/issues/10239

#### Configuracion http+https

(WIP) comandos sep o join para no usar editor

#### Configuracion http-lighty->proxy->http-apache

En esta configuracion instalamos apache2 y lighty en la misma maquina, pero el servicio http 
es servido solo por lighttpd, mientras que el apache2 queda solo para sistemas que solo ejecuten en el, 
para poder usarlo asi, se configura el lighty como servidor principal, y apache2 en otros puertos, 
la conexcion o el como se sirven los archivos se realiza redireccionando con el modulo proxy.

El modulo proxy actua como un gateway, como una pasarela, un hombre en el medio, toma todo 
paquete que viene al lighttpd y lo reenvia al apache, pero a vista del cliente en su navegador 
este no notara nada, porque esta el lighttpd gestionando esta redireccion.

(WIP)

## php y lighty

Una vez instalado tenemos lighty ejecutando, entonces le adicionamos soporte php, 
en alpine tenemos dos ramas de php entre las cuales tenemos 5 y 7 en las versiones 
siguentes php 5.6, php 7.1, php 7.2 y solo en las ultimas php 7.3 no muy compatible.

#### php 5.6

Este es el mas compatible con todo software y la mas efectiva, 
**OJO: php5 5.6 esta en el repositorio main para todos los alpines**

```
apk install php5 php5-cli php5-fpm php5-cgi php5-sockets php5-phar php5-iconv php5-curl php5-pear

# sed 'include "mod_fastcgi.conf"' (WIP)

echo "<?php echo phpinfo();?>" > /var/www/localhost/htdocs/index.php

rc-service lighttpd restart
curl http://127.0.0.1/index.php | grep engine
```

Veras en la consola la palara "Zend engine" despues del ultimo comando si todo marcha bien.

***Pero, pero pero, tranca palanca** esta es una manera muy bien integrada 
pero solo optimizada para paginas pequeñas, para servicios de alto volumen pero archivos pequeños 
se recomienda el `php-fpm` (en este caso `php5-fpm` junto con `mod_proxy.conf`), 
ojo para paginas sean grandes o pequeñas pero archivos grandes no usar `php-fpm`!

```
apk add php5-fpm
```

Despues hay que editar el archivo `/etc/lighttpd/mod_fastcgi.conf` y especificar el php a 5 asi:

Ahora reiniciar los nuevos servicios:

```
rc-service lighttpd restart
rc-update add php5-fpm
```

#### php 7

Esta es la que los de php nos estan obligando/forzando usar sin necesidad alguna:
**OJO: php7 7.1/7.2 esta en el repositorio contriution solamente**

```
apk install php7 php7-cli php7-fpm php7-cgi php7-sockets php7-phar php7-iconv php7-curl php7-pear

echo "<?php echo phpinfo();?>" > /var/www/localhost/htdocs/index.php

curl http://127.0.0.1/index.php | grep engine
```

Veras en la consola la palara "Zend engine" despues del ultimo comando si todo marcha bien.

#### Lista de paquetes php

Por orden de necesidad, separados por utilidad y prioridad sobre otros:

* php5-common
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


