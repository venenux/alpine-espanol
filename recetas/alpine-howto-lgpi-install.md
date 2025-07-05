# alpine + lgpi

LGPI es el Administrador de Recursos de Información con una interfaz gráfica de 
usuario adicional para tareas administrativas.
Puede usarlo para crear una base de datos con el inventario de su empresa, por 
ejemplo, computadoras, software, impresoras, etc. Cuenta con funciones 
mejoradas para facilitar la vida diaria de los administradores, como un sistema 
de seguimiento de trabajos con notificaciones por correo electrónico y métodos 
para crear una base de datos con información básica sobre la topología de su red.

1. El inventario preciso de todos los recursos técnicos almacenados en una base de datos.
2. Gestión e historial de las acciones de mantenimiento y los procedimientos vinculados.

Esta aplicación es dinámica y está conectada directamente con los usuarios, 
quienes pueden enviar solicitudes a los técnicos. Una interfaz les permite, si 
es necesario, detener el servicio de mantenimiento e indexar un problema 
detectado con uno de los recursos técnicos a los que tienen acceso.

Este material está sujeto a derechos de autor. Consulte la [LICENCIA](#license) 
al final del documento. y puedes ver el video en https://t.me/alpine_linux/1402

La instalacion aqui no es robando todo el dominio, nuestra instalacion es modo 
flexible, en vez de tomar toda un raiz, solo usa una subruta del url del server 
(es decir, en un subdirectorio). Adicional emplea un alias, asi que no se coloca 
en el directorio raiz del webserver, sino en las webapps de alpine como debe ser.

* [Install alpine linux](#install-alpine-linux)
* [1 - Environment](#1---setup-environment)
* [2 - Download and setup webmin](#2---download-and-setup-webmin)
* [3 - Configuration for modules](#3---configuration-for-modules)
  * [Problems and fails](#problems-and-fails)
* [4 - Full automated way](#4---full-automated-way)
* [5 - basic webmin modules]()
* [How to use this guide](#how-to-use-this-guide)
* [LICENSE](#LICENSE)

Este documento es uan receta directa a seguir, porque funciona y funciona muy 
bien. Revisa [How to use this guide](#how-to-use-this-guide) antes de comenzar.

## Install alpine linux

> **Warning**: Si ya tienes Alpine Running, salta y ve a [0 - Environment](#0---setup-environment) !

Estos comandos son para cualquier release. Crean un disco y ejecutan una 
máquina virtual para instalar Alpine Linux 3.20 como sistema operativo base 
para la configuración de SNIPE-IT.

Puede usar Alpine 3.20, 3.22 o Edge. Cualquier Alpine funcionará a partir de 
la versión 3.20 para instalar el sistema SNIPE-IT. En este documento, solo 
usamos la versión 3.22.

> **Warning** necesitaras cambiar lso nombres de los paquetes php en las viejas

```
mkdir -p /home/general/VM/alpine322 && cd /home/general/VM/alpine322

qemu-img create -f raw computerint1alpine-vitualdisk1-file.raw 6G

wget -c -t8 --no-check-certificate http://dl-cdn.alpinelinux.org/alpine/v3.22/releases/x86_64/alpine-extended-3.22.0-x86_64.iso

/usr/bin/qemu-system-x86_64  -m 2048 -name "computerint1alpine322" \
 -cpu host -machine q35 \
 -device virtio-net,netdev=nd1 -netdev user,id=nd1,restrict=off,hostfwd=tcp::3222-:22,hostfwd=tcp::9080-:80,hostfwd=tcp::9443-:443 \
 -device virtio-keyboard -device virtio-mouse -device virtio-tablet -device virtio-vga,max_outputs=1 \
 -drive file=computerint1alpine-vitualdisk1-file.raw,format=raw \
 -cdrom alpine-extended-3.22.0-x86_64.iso -boot order=cd,once=d,menu=off
```

* Al iniciarlo, solicitará root, solo escriba "root" y presione Enter para 
iniciar el comando`setup-alpine`

#### the setup-alpine command procedure

* keyboard; por ejemplo, para latín es "es" y después "es-winkeys".
* hostname: simplemente pulsa Intro; es el nombre del equipo; solo deben ser cadenas.
* network: selecciona eth0, que es el cable de red, y responde DHCP.
* network (de nuevo): solo ocurre si tienes wifi o una segunda tarjeta
* DNS options: Se recomienda usar 8.8.8.8 y "none" para el dominio.
* root: Contraseña para la cuenta administrativa; ten cuidado.
* timezone: usa UTC asi de simple; de lo contrario, usa América/Caracas o similar.
* proxy: No uses "none" si te conectas directamente a internet.
* NTP: Usa "chrony" para el paquete que ya está en el medio (extendido).
* mirror APK: Si su internet es lenta o no funciona, escriba "omit" o "none".
* user: versiones modernas de Alpine permiten la creación de usuarios; escriba "No".
* SSH: Use el paquete openssh que viene en el medio (extendido).
* SSH root: Aquí debe escribir "Sí", ya que aún no se configura el usuario.
* SSH heys: Escriba "none".
* Opciones de disco: Use "sda", ya que se usará todo el disco duro.
* Modo: Seleccione "sys" para instalar el sistema en el disco.

Luego reinicie y tenga cuidado de la direccion IP de la máquina virtual.

## 1 - Setup environment

No omita ni omita ningún paquete o comando, ya que glpi silenciará los fallos.
Esta primera parte configurará los repositorios principal y comunitario, luego 
instalará las dependencias para la instalación y también programas base.
Por último, configurará la fuente de la consola. Si no lo sabe, 
consulte [Cómo usar esta guía](#how-to-use-this-guide). ahora ejecute todo lo 
siguiente como usuario root asi:

```
cat > /etc/apk/repositories << EOF
http://dl-4.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-4.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update

apk add openssl perl perl-net-ssleay perl-io-socket-ssl perl-io-tty \
 perl-datetime perl-datetime-timezone perl-datetime-locale attr diffutils \
 at dcron man-pages nano binutils coreutils readline shared-mime-info \
 grep gawk sed attr dialog lsof less groff procps wget curl terminus-font \
 file findutils gawk tree pciutils usbutils lshw tzdata tzdata-utils \
 zip unzip p7zip xz tar cabextract cpio binutils lha gzip lz4 \
 ethtool musl-locales musl-locales-lang  arch-install-scripts util-linux \
 docs iproute2-minimal psmisc net-tools lsof curl wget apkbuild-cpan

rc-update add consolefont boot


cat > /etc/hosts << EOF
127.0.0.1	provision.domino.ver provision localhost.localdomain localhost
::1		localhost localhost.localdomain
EOF

echo "provision.domino.ver"
```

> **Warning**: execute all the commands before, if you dont know how to, check [How to use this guide](#how-to-use-this-guide)

## 2 - Install prerequisites

GLPI es una aplicación web que requiere: un servidor web, PHP y una base de 
datos. Para ello, continúe como usuario root y ejecute los siguientes comandos:

```
apk add sed apache2 apache2-utils apache2-error apache2-ssl apache2-ctl

mkdir -p /var/www/localhost/htdocs /var/log/apache2
sed -i -r 's#^Listen.*#Listen 80#g' /etc/apache2/httpd.conf
sed -i -r 's#^ServerTokens.*#ServerTokens Minimal#g' /etc/apache2/httpd.conf
sed -i -r 's#.*LoadModule.*modules/mod_alias.so.*#LoadModule alias_module modules/mod_alias.so#g' /etc/apache2/httpd.conf
sed -i -r 's#.*LoadModule.*modules/mod_rewrite.so.*#LoadModule rewrite_module modules/mod_rewrite.so#g' /etc/apache2/httpd.conf
chown -R apache:www-data /var/www/localhost/
chown -R apache:wheel /var/log/apache2
echo "it works" > /var/www/localhost/htdocs/index.html
rc-update add apache2 default

IP=$(ip a s | grep 'inet ' | grep -Eo '([0-9]*\.){3}[0-9]*' | grep -v '127.0' | head -n1)
HN=$(hostname)
echo "$IP $HN" >> /etc/hosts

rc-service apache2 restart
```

ahora configurar y colocar php

```
apk add php82-opcache php82-openssl php82-json php82-bcmath php82-mbstring \
 php82-bz2 php82-zip php82-tidy php82-calendar php82-intl php82-pspell \
 php82-ctype php82-dev php82-dom php82-enchant php82-fileinfo php82-shmop \
 php82-simplexml php82-dom php82-sysvmsg php82-sysvsem php82-sysvshm \
 php82-tokenizer php82-xml php82-xmlreader php82-iconv \
 php82-xmlwriter php82-xsl php82-xmlwriter php82-sodium \
 php82-exif php82-gd php82-pcntl php82-mysqli php82-pdo php82-pdo_mysql \
 php82-sockets php82-curl php82-pear php82-phar php82-session \
 php82 php82-apache2 

sed -i -r 's|.*cgi.fix_pathinfo=.*|cgi.fix_pathinfo=1|g' /etc/php*/php.ini
sed -i -r 's#.*safe_mode =.*#safe_mode = Off#g' /etc/php*/php.ini
sed -i -r 's#.*expose_php =.*#expose_php = Off#g' /etc/php*/php.ini
sed -i -r 's#memory_limit =.*#memory_limit = 536M#g' /etc/php*/php.ini
sed -i -r 's#upload_max_filesize =.*#upload_max_filesize = 128M#g' /etc/php*/php.ini
sed -i -r 's#post_max_size =.*#post_max_size = 256M#g' /etc/php*/php.ini
sed -i -r 's#^file_uploads =.*#file_uploads = On#g' /etc/php*/php.ini
sed -i -r 's#^max_file_uploads =.*#max_file_uploads = 12#g' /etc/php*/php.ini
sed -i -r 's#^allow_url_fopen = .*#allow_url_fopen = On#g' /etc/php*/php.ini
sed -i -r 's#^.default_charset =.*#default_charset = "UTF-8"#g' /etc/php*/php.ini
sed -i -r 's#^.max_execution_time =.*#max_execution_time = 150#g' /etc/php*/php.ini
sed -i -r 's#^max_input_time =.*#max_input_time = 90#g' /etc/php*/php.ini
sed -i -r 's#^session.cookie_secure =.*#session.cookie_secure = on#g' /etc/php*/php.ini
sed -i -r 's#^session.cookie_samesite =.*#session.cookie_samesite = Lax#g' /etc/php*/php.ini

cat > /var/www/localhost/htdocs/index.php << EOF 
<?php
phpinfo();
?>
EOF
```

Ahora configurar la base de datos

```
apk add tzdata mysql mysql-client mariadb-doc mariadb-server-utils

mysql_install_db --user=mysql --datadir=/var/lib/mysql

rc-service mariadb start

mysqladmin -u root password toor1

cat > /etc/my.cnf.d/mariadb-timezone.cnf << EOF
[mysqld]
default-time-zone = 'America/Panama'
EOF
mysql_tzinfo_to_sql /usr/share/zoneinfo/ | mysql -u root -ptoor1 mysql

sed -i "s|.*max_allowed_packet\s*=.*|max_allowed_packet = 100M|g" /etc/mysql/my.cnf
sed -i "s|.*max_allowed_packet\s*=.*|max_allowed_packet = 100M|g" /etc/my.cnf.d/mariadb-server.cnf
sed -i "s|.*bind-address\s*=.*|bind-address=0.0.0.0|g" /etc/mysql/my.cnf
sed -i "s|.*bind-address\s*=.*|bind-address=0.0.0.0|g" /etc/my.cnf.d/mariadb-server.cnf
sed -i "s|.*skip-networking.*|#skip-networking|g" /etc/mysql/my.cnf
sed -i "s|.*skip-networking.*|#skip-networking|g" /etc/my.cnf.d/mariadb-server.cnf

rc-service mariadb restart

rc-update add mariadb

mkdir -p /usr/share/webapps/adminer

wget https://github.com/vrana/adminer/releases/download/v5.3.0/adminer-5.3.0.php -O /usr/share/webapps/adminer/adminer-5.3.php

rm -rf /usr/share/webapps/adminer/index.php
ln -s adminer-5.3.php /usr/share/webapps/adminer/index.php

cat > /etc/apache2/conf.d/adminer.conf << EOF
Alias /adminer /usr/share/webapps/adminer/
<Directory /usr/share/webapps/adminer/>
    Require all granted
    DirectoryIndex index.php
</Directory>
EOF

rc-service apache2 restart
```

## 3 - Download and setup GLPI

GLPI no tiene una forma de automatizar la instalación.

```
apk add aria2

cd /tmp

aria2c https://github.com/glpi-project/glpi/releases/download/10.0.18/glpi-10.0.18.tgz

tar zxvf glpi-10.0.18.tgz

mkdir -p /usr/share/webapps && rm -rf /usr/share/webapps/glpi

mv glpi /usr/share/webapps/

cat > /etc/apache2/conf.d/glpi.conf << EOF
Alias /glpi /usr/share/webapps/glpi/
<Directory /usr/share/webapps/glpi/>
    Require all granted
    DirectoryIndex index.php
</Directory>
EOF

cat > /usr/share/webapps/glpi/public/.htaccess << EOF
RewriteBase /
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)\$ index.php [QSA,L]
EOF

sed -i '3,$ s/^#//' /usr/share/webapps/glpi/.htaccess

chown -R apache:www-data /usr/share/webapps/glpi/files
chown -R apache:www-data /usr/share/webapps/glpi/config

rc-service apache2 restart
```

Ahora configurar la base de datos y el entorno antes de conectarla

```
mysql -u root -ptoor -e "CREATE DATABASE glpi;"
mysql -u root -proot -e "CREATE USER 'glpiudb'@'%' IDENTIFIED BY 'glpi.1';FLUSH PRIVILEGES;"
mysql -u root -proot -e "CREATE USER 'glpiudb'@'localhost' IDENTIFIED BY 'glpi.1';FLUSH PRIVILEGES;"
mysql -u root -ptoor -e "GRANT ALL PRIVILEGES ON glpi.* TO 'glpiudb';"
mysql -u root -ptoor -e "GRANT ALL PRIVILEGES ON glpi.* TO 'glpiudb'@'localhost' WITH GRANT OPTION;"
mysql -u root -ptoor -e "GRANT SELECT ON mysql.time_zone_name TO 'glpiudb'@'localhost';"

cd /usr/share/webapps/glpi

php82 bin/console db:install -f --default-language=es_VE --no-telemetry -H localhost -d glpi -u glpiudb -p glpi.1

php bin/console task:unlock --all
```

Ahora puede visitar http://provision.domino.ver/glpi , Los usuarios son:

* glpi/glpi admin account,
* tech/tech technical account,
* normal/normal “normal” account,
* post-only/postonly post-only accoun

## 4 - Configuration for remote access


La instalación solo cubre la administración de la interfaz web, pero para tener 
acceso completo, además de la interfaz web, necesitará configurar SSH. Para 
ello, ejecute los siguientes comandos para configurar el software SSH en el 
servidor y luego corrija cualquier parámetro adicional, por ejemplo, al 
instalar módulos adicionales del sistema. Si no sabe cómo usar esta guía, 
consulte la sección [Cómo usar esta guía](#how-to-use-this-guide). De lo 
contrario, ejecute todos los siguientes comandos como usuario root:

```
apk add doas bash shadow shadow-uidmap musl-locales musl-locales-lang \
 e2fsprogs btrfs-progs exfat-utils f2fs-tools dosfstools xfsprogs jfsutils zfs \
 acpi patch coreutils mdadm e2fsprogs-extra attr smartmontools doas-sudo-shim \
 iproute2 netpbm poppler-utils libjpeg-turbo-utils perl-socket6 libqrencode-tools

cat > /etc/doas.d/apkgeneral.conf << EOF
permit  general as root cmd apk
permit  general as root cmd service
EOF

useradd -m -U -c "" -s /bin/bash -G wheel,input,disk,floppy,cdrom,dialout,audio,video,lp,netdev,games,users,ping,wheel general

for u in $(ls /home); do for g in disk lp floppy audio cdrom dialout video lp netdev games users ping wheel; do addgroup $u $g; done;done

echo "general:general.1" | chpasswd

sed -i -r 's|#PermitRootLogin.*|PermitRootLogin no|g' /etc/ssh/sshd_config

echo AllowUsers general >> /etc/ssh/sshd_config

service sshd restart
```

Ahora todo usará el usuario "general", así que ejecute "su -l general" o inicie 
sesión con el usuario general (la misma contraseña, por si acaso). Este usuario 
podrá instalar paquetes y reiniciar servicios como superusuario. Además, a 
partir de ahora será el único usuario con acceso a la máquina mediante SSH.

## How to use this guide

Esta guía **estructura todos los comandos en bloques, cada bloque separado por 
una línea**, por lo que debe **escribir cada línea tal cual... y pulsar Intro**. 
Como ya habrá notado, al escribir cada bloque de comandos por separado, 
copie/escriba solo bloques separados por una línea vacía.
Todas las líneas nuevas (siguientes) se crean con solo Intro. La terminal 
detectará si se debe ejecutar o no.

Esta guía se centra en el proceso de instalación; muchas partes requieren conocimientos mínimos de Linux.

Esta guía asume que tiene un puerto serie habilitado en el ordenador de destino. También es importante que comprenda la configuración de esta guía.

> **Warning**  Algunas terminales Linux y/o Mac tienen bloqueos de seguridad para cortar y pegar, por lo que si pega, la primera línea estará precedida de basura. Compruebe siempre el primer carácter de su texto pegado.

## see also

- 🗯 IRC
  - 💬 `##alpine_telegram_english`
  - 💬 `#alpine_linux_english`
- 📱 Telegram https://t.me/alpine_linux
  - 🇬🇧 https://t.me/alpine_linux_english
  - 🇷🇺 https://t.me/alpine_linux_pycckuu (dual english russian, low activity)
  - 🇨🇴 https://t.me/alpine_linux_espanol
  - 🇧🇬 https://t.me/alpine_linux_bulgarian (dual english bulgarian, low activity)
  - 🇨🇳 https://t.me/alpine_linux_chinese (dual english chinese, low activity)
  - 📡 https://t.me/opentechnologies (open languajes but english as main)
- Matrix
  - 👥 https://matrix.to/#/#alpine-linux-english:matrix.org

# LICENSE

**CC BY-NC-SA**: El proyecto permite a los reutilizadores distribuir, remezclar, 
adaptar y desarrollar el material en cualquier medio o formato, exclusivamente 
con fines no comerciales, y siempre que se atribuya la autoría a los creadores 
involucrados. Si remezcla, adapta o desarrolla el material, debe licenciar el 
material modificado bajo términos idénticos, incluyendo los siguientes 
elementos:
* **BY**  – Se debe dar crédito al creador de cada contenido respectivamente, comenzando por el primer colaborador.
* **NC**  – Sólo se permiten usos no comerciales de la obra, ¡con excepción si rellenas un formulario aquí!
* **SA**  – Las adaptaciones deben compartirse bajo los mismos términos, debes obedecer estos términos y no cambiarlos.