# Alpine y sistema de paquetamiento

Este documento instruye como construir paquetes para alpine linux, 
si desea amplia la informacion esta guia esta igual pero con explicaciones 
en la seccion de documentos en [../documentos/alpine-paquetes-entorno.md](../documentos/alpine-paquetes-entorno.md).

- [Instalacion y usuario del entorno abuild](#instalacion-y-usuario-del-entorno-abuild)
- [Creacion del usuario y entorno abuild](#creacion-del-usuario-y-entorno-abuild)
- [Iniciando sesion y configurando el entorno](#iniciando-sesion-y-configurando-el-entorno)
- [Crear un paquete nuevo con internet](#crear-un-paquete-nuevo-con-internet)
- [Mas informacion](#mas-informacion)

1. Cada linea es un comando que puede ejecutarse, si hay varias lineas seguidas, 
se puede pegar todas estas lineas juntas en la consola y los comandos se ejecutaran
pero, cada dos lineas en blanco significa que debe esperar a que termine el 
comando para ejecutar el siguente, por ende si hay dos lineas separadas por una 
linea en blanco o vacia, significa que debe pegar la primera y depsues la segunda 
solo si la primera ya se ejecuto.
2. Todos los comandos estan diseniados de forma que puede siempre ejecutarlos asi 
ya hayan realizado su funcion, no afectara volver ejecutarlos.
3. Notese que cada apartado de las listas tiene un grupo de comandos que 
estan separados por una linea vacia, significa que no solo explica cada grupo 
y que hace sino que tambien indica que debe esperar que este grupo de comandos 
se hayan ejecutado correctamente antes de "pegar" el siguente lote a ejecutar.


### Instalacion y usuario del entorno abuild

```bash
apk add shadow shadow-doc shadow-uidmap bash bash-doc bash-dev \
 doas doas-doc doas-sudo-shim coreutils coreutils-doc tree tree-doc \
 man-db man-pages zlib zlib-doc wget wget-doc curl curl-doc aria2 aria2-doc \
 sed sed-doc lsof lsof-doc less less-doc groff groff-doc gawk gawk-doc \
 zip zip-doc p7zip p7zip-doc xz xz-doc tar tar-doc file file-doc
 
apk add abuild abuild-rootbld abuild-doc build-base gcc-doc make-doc patch-doc \
 gcc gcc-gdc gcc-go g++ gcc-objc gcc-doc \
 arch-install-scripts arch-install-scripts-doc lzip-doc tar-doc zlib-doc \
 apk-tools-doc alpine-sdk git git-doc

apk add doas doas-sudo-shim
```

La justificacion de estos paquetes puede leerla en 
[../documentos/alpine-paquetes-entorno.md seccion paquetes y repositorios](../documentos/alpine-paquetes-entorno.md#2---practica---hacer-paquetes-y-repositorios).

### Creacion del usuario y entorno abuild

```bash
useradd -m -U -c "" -G abuild,wheel,input,disk,floppy,cdrom,dialout,audio,video,lp,netdev,games,users,ping general

cat >  /etc/doas.d/general.conf << EOF
permit keepenv :general
EOF

for u in $(ls /home); do for g in abuild wheel adm netdev kvm disk lp floppy audio cdrom dialout video lp games users ping; do addgroup $u $g; done;done
```

La explicacion de que hacen y el porque estos comandos puede leerla en 
[../documentos/alpine-paquetes-entorno.md seccion creacion del usuario y entorno](../documentos/alpine-paquetes-entorno.md#creacion-del-usuario y-entorno-abuild).

Ahora **cierre la sesión de la cuenta de root, e inicie sesión como `general`**. 

### Iniciando sesion y configurando el entorno

User must be in some special groups what will be only available if you follow this guide from beggining!

```
mkdir -m 775 -p /home/general/Devel
chown general:abuild /home/general/Devel

git config --global pull.rebase=true
git config --global ssh.postBuffer 2000000000
git config --global http.postBuffer 2000000000
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999
git config --global https.postBuffer 2000000000
git config --global https.lowSpeedLimit 0
git config --global https.lowSpeedTime 999999

doas mkdir -m 775 -p /var/cache/distfiles
doas chgrp abuild /var/cache/distfiles
doas sed -i 's|export CFLAGS\s*=.*|export CFLAGS="-O2"|g' /etc/abuild.conf
doas sed -i 's|.*USE_CCACHE\s*=.*|USE_CCACHE=1|g' /etc/abuild.conf
doas sed -i 's|SRCDEST\s*=.*|SRCDEST=/var/cache/distfiles|g' /etc/abuild.conf
doas mkdir -m 775 -p /home/general/Devel/packages
doas chown general:abuild /home/general/Devel
doas sed -i 's|REPODEST\s*=.*|REPODEST=\$HOME/Devel/packages/|g' /etc/abuild.conf

git config --global user.email "general@venenux.xxx"
git config --global user.name "generalvenenux"
doas sed -i 's|.*PACKAGER\s*=.*|PACKAGER="generalvenenux <general@venenux.xxx>"|g' /etc/abuild.conf
doas sed -i 's|.*MAINTAINER\s*=.*|MAINTAINER="\$PACKAGER"|g' /etc/abuild.conf

abuild-keygen -a -i -n
```

La explicacion de que hacen y el porque estos comandos puede leerla en 
[../documentos/alpine-paquetes-entorno.md seccion inicio del usuario aqui](../documentos/alpine-paquetes-entorno.md#iniciando-sesion-y-configurando-el-entorno).

### Crear un paquete nuevo con internet

* Crear un directorio de trabajo y cambiarse a dicho directorio
* Crear el archivo APKBUILD con la ayuda del sistema de desarrollo abuild
* Se creara el directorio con el nombre del paquete
* Despues edite el archivo `APKBUILD` segun las opciones, para que empaquete
* Despues de ajustar se actualiza en checksum del `APKBUILD` para generarlo
* al compilar el resultado estara en el directorio `/home/general/Devel/packages/`

```bash
mkdir -p /home/general/Devel/alpine-apkbuilds/main

cd /home/general/Devel/alpine-apkbuilds/main

newapkbuild -f -d "crea dialogos segun el entorno" -n easybashgui -l MIT -a https://github.com/BashGui/easybashgui/archive/refs/tags/13.0.0.tar.gz

cd /home/general/Devel/alpine-apkbuilds/main/easybashgui
echo "aqui editar los archivos presentes y salvar"

abuild checksum

abuild -r
```

Si no tiene internet y ya tiene las fuentes en local en el mismo directorio, 
lease el caso [Ejercicio 2 crear un paquete nuevo con las fuentes local](../documentos/alpine-paquetes-entorno.md#ejercicio-2-crear-un-paquete-nuevo-con-las-fuentes-local) 
en la guia extendida de la documentacion donde se explica mucho mas detallado.

Si se compilo, el nuevo binario estara en `/home/general/Devel/packages/main`, 
y se creara un directorio por cada arquitectura, siendo estos `x86`, `x86_64`, 
`armv7` etc

# Mas informacion

Esta guia es una version resumida de el proceso de creacion de paquetes, si 
necesita **mejores pasos o mas detallados** revise [../documentos/alpine-paquetes-entorno.md](../documentos/alpine-paquetes-entorno.md).

Quiere **publicar el repositorio en servidor web**, lease la guia extendida en 
[../documentos/alpine-paquetes-entorno.md seccion anexos publicar el repositorio](../documentos/alpine-paquetes-entorno.md#generacion-automatica-de-mi-repositorio).

- Author
  - Sodomon as https://t.me/alpine_linux_espanol/26413
  - mckaygerhar PICCORO Lenz McKAY
- 🗯 IRC
  - 💬 `##alpine_telegram_english` (roto)
- 📱 Telegram https://t.me/alpine_linux
  - 🇨🇴 https://t.me/alpine_linux_espanol
  - 📡 https://t.me/latam_programadores
- Matrix
  - 👥 https://matrix.to/#/#alpine-linux-espanol:matrix.org

# LICENSE

**CC BY-NC-SA** Sodomon 2020
