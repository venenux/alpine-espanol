# Alpine y sistema de paquetamiento

El procedimiento implica tener cuatro cosas en tu linux alpine, que 
son el navegador web, el programa git y las utilidades de compilacion.

Este documento se divira en dos partes, la primera la informacion teorica, 
y la segunda la forma practica y configuraciones que debera realizar.

Una version reducida y directa de este documento esta en la guia
[../recetas/alpine-recetas-hacer-paquetes-alpine-localmente.md](../recetas/alpine-recetas-hacer-paquetes-alpine-localmente.md)

- [1 - Teoria - Sistema de paqueteria alpine linux](#1---teoria---sistema-de-paqueteria-alpine-linux)
    - [empaquetar](#empaquetar)
    - [formato APK](#formato-apk)
    - [compilacion](#compilacion)
    - [repositorios](#repositorios)
    - [manejador de paquetes](#manejador-de-paquetes)
- [2 - Practica - Hacer paquetes y repositorios](#2---practica---hacer-paquetes-y-repositorios)
    - [Instalacion de los programas necesarios](#instalacion-de-los-programas-necesarios)
    - [Creacion del usuario y entorno abuild](#creacion-del-usuario-y-entorno-abuild)
    - [Iniciando sesion y configurando el entorno](#iniciando-sesion-y-configurando-el-entorno)
    - [Ejercicio 1 crear un paquete nuevo con internet](#ejercicio-1-crear-un-paquete-nuevo-con-internet)
    - [Ejercicio 2 crear un paquete nuevo con las fuentes local](#ejercicio-2-crear-un-paquete-nuevo-con-las-fuentes-local)
    - [Generacion automatica de mi repositorio](#generacion-automatica-de-mi-repositorio)
- [Anexos ](#anexos-)
    - [Como se creaban los APKBUILD](#como-se-creaban-los-apkbuild)
    - [sobre abuild-keygen](#sobre-abuild-keygen)
    - [Generacion manual de repositorio](#generacion-manual-de-repositorio)
- [mas informacion](#mas-informacion)
- [LICENSE](#license)


## 1 - Teoria - Sistema de paqueteria alpine linux

El **sistema de paqueteria** de alpine consta de dos lados, la **fabricacion** 
(empaquetar y compilar) y la **publicacion** (los repositorios). El sistema 
de paqueteria **en realidad es enteramente usando bash**.

#### empaquetar

Se compilan programas usando versiones especificas, estos se comprimen en 
archivos tar firmados digitalmente, se les agrega metadatos de informacion 
que conteine las dependencias y los archivos de configuracion. A este 
resultado se le denomina **formato de paquetes APK**.

#### formato APK

**APK** es el formato llamado "a-packs", su estructura consta de dos partes: 

* **.PKGINFO** el archivo que contiene la metadata de informacion (la cual no es 
mucha, por lo que es hackeable) por ejemplo el paquete "bash-4.0-r3" en la info 
solo estara la parte "4.0" y el "r3" esta en el normbre del archivo del paquete. 
Este archivo contiene tambien la arquitectura, el comit del repo alpine donde 
se realizo el paquete (lamentablemente enlazado a aports), asi como la licensia 
y la arquitectura de la compilacion de los archivos.
* **TAR** el archivo que contiene el software compilado empaquetado, es un 
archivo tar pero cadicionalmente se comprime, la extension/tipo compresion no 
es especifica y puede variar, adentro esta la estructura exacta de donde estaria 
colocado cada uno de los archivos en el sistema operativo. Solo estaran incluidos 
los archivos relativos a la compilacion de lo que indica el archivo de informacion 
de metadata descrito anteriormente.

#### compilacion

**APKBUILDS** es el formato y archivo que define como se creara los paquetes, 
este **es simplemente un archivo bash que es usado en modo de inclusion**, 
este es el unico necesario y todo lo demas es referenciado (local o desde internet) 
desde direcciones en este. Este archivo es en realidad una secuencia de reglas, 
que se ejecutan solo si estan depsues de uan cabecera, esta informacion tampoco 
estan en la (ya muy porqueria) wiki oficial de alpine, sino que solo describen 
los elementos tecnicos que lo caracterizan.

La **estructura** una **cabecera informativa** (la base para generar el `.PKGINFO`, 
son meras variables) y **algunas de funciones** (que se modifican segun el 
software que se va empaquetar) haciendo de **secuencia de reglas**.

La **secuencia de reglas**: revision (`sanitycheck() -> clean()-> fetch() -> verify()`) 
depsues de corroborar se procede a desempaquetar y preparar la estructura del 
softwaare segun las reglas (`-> unpack() -> prepare() -> mkusers()`), para poder 
despues compenzar a compilar el programa (`-> build() -> check()`) si este compila 
entonces procede a crear el archivo APK segun las indicaciones del APKBUILD y 
si hay detalles extra (`-> package() -> subpackages() -> apk -> cleanup()`).

La **aplicacion de empaquetado** se llama `abuild` y se encarga de ejecutar esta 
secuencia de reglas y sus funcionalidades..

Estos paquetes APK generados despues se colocan en **repositorios de paquetes**.

#### repositorios

Los paquetes se **agregan en un unico directorio** (sin organizacion, todos 
directos en la raiz del directorio), y se aniade al mismo un **archivo indice**, 
por lo que **cualquier directorio con una coleccion de archivos APK y un indice 
es un repositorio alpine.**

El **archivo indice** es llamado `APKINDEX` y contiene la misma informacion que 
la que esta en `.PKGINFO` pero de todos los paquetes juntos, separadas por una 
linea extra en blanco. Este archivo siempre esta comprimido generalmente tendra 
el nombre de `APKINDEX.tar.gz`

Los directorios del repositorio indican una **rama de paquetes**, alpine desde 
la version 3 tiene dos ramas, la **main** y la **community**, los repositorios 
tiene estos dos directorios y cada uno tiene todos los paquetes sin ningun 
subdirectorio. En cada rama directorio esta un archivo indice `APKINDEX` compreso.

En la actualidad cuando se construye localmente un paquete, un repositorio 
se genera y se crea su indice autoatico en su home. Remitase a la configuracion 
de `abuild` en la segunda seccion en la parte de construccion y repositorio 
de paquetes.

#### manejador de paquetes

Para poder emplear estos se utiliza la utilidad de manejo de paquetes **apk** 
el cual tiene el mismo nombre que el formato y que el tipo de paquetes.
Para mayor informacion de como utilizarlo lea la pagina wiki [apk-tool.md](apk-tool.md)

## 2 - Practica - Hacer paquetes y repositorios

Aqui se aprendera crear y hacer un repositorio de `APKBUILD`s (recetas) 
para hacer paquetes para [Alpine linux](../README.md)
tambien para configurar `abuild` para que puedas crear los paquetes 
a partir de los `APKBUILD`

Una vez esto podras ejecutar estas recetas (los `APKBUILD`s) y entonces despues 
publicar (en los `repositorios`) lo producido (los paquetes `apk`s binarios).

**ADVERTENCIA** los "alpinistas" son fanaticos de no colocar nada, lo siendo, 
aqui se va trabajar con las herramentas correctas, asi que instale lo que se 
indica aqui! Que le quede claro.

- [Instalacion de los programas necesarios](#instalacion-de-los-programas-necesarios)
- [Creacion del usuario y entorno abuild](#creacion-del-usuario-y-entorno-abuild)
- [Iniciando sesion y configurando el entorno](#iniciando-sesion-y-configurando-el-entorno)
- [Ejercicio 1 crear un paquete nuevo con internet](#ejercicio-1-crear-un-paquete-nuevo-con-internet)
- [Ejercicio 2 crear un paquete nuevo con las fuentes local](#ejercicio-2-crear-un-paquete-nuevo-con-las-fuentes-local)
- [Generacion automatica de mi repositorio](#generacion-automatica-de-mi-repositorio)

Cada linea es un comando que puede ejecutarse, si hay varias lineas seguidas, 
se puede pegar todas estas lineas juntas en la consola y los comandos se ejecutaran
pero, cada dos lineas en blanco significa que debe esperar a que termine el 
comando para ejecutar el siguente, por ende si hay dos lineas separadas por una 
linea en blanco o vacia, significa que debe pegar la primera y depsues la segunda 
solo si la primera ya se ejecuto.

### Instalacion de los programas necesarios

Primero necesitamos el entorno base de empaquetamiento o desarrollo, implica:

* utilidades base del sistema, entre ellas bash aunque no lo use y archivadores
* programas de trabajo, entre ellos abuild, git, y gnu/compilers.

```bash
apk add shadow shadow-doc shadow-uidmap bash bash-doc bash-dev \
 doas doas-doc doas-sudo-shim coreutils coreutils-doc tree tree-doc \
 man-db man-pages zlib zlib-doc wget wget-doc curl curl-doc aria2 aria2-doc \
 sed sed-doc lsof lsof-doc less less-doc groff groff-doc gawk gawk-doc \
 zip zip-doc p7zip p7zip-doc xz xz-doc tar tar-doc file file-doc
 
apk add abuild abuild-rootbld abuild-doc build-base gcc-doc make-doc patch-doc \
 arch-install-scripts arch-install-scripts-doc lzip-doc tar-doc zlib-doc \
 apk-tools-doc alpine-sdk git git-doc
```

Tenga en claro, que entre los comandos, se agregan tambien los paquetes de 
las paginas de manual ( los `xxx-doc` y el `man-db` y `man-pages`), esto es 
imperativo porque lo va necesitar para corroborar las opciones de los programas 
asi como datos del mismo, por ejemplo inclusion de ejemplos de los mismos.

Algunos paquetes se incluyen **para evitar fallas en el software empaquetar**, 
especialmente si ud no ha usado alpine antes o es la primera vez que empaquetara, 
ya que todo programa que empaquete no tendra jamas en cuenta a la distro alpine.

### Creacion del usuario y entorno abuild

El entorno de creacion necesita realizarse sea con un usuario dedicado o desde 
su usuario de uso normal, en VenenuX alpine no se permiten usuarios personalizados 
ya que la diversidad que caracteriza linux es la misma que causa sus problemas, 
usando el mismo usuario estandarizado todos nuestros scripts funcionaran.

El entorno se realiza sin ser administrador (el usuario `root`) dado que si estos 
programas que empaqueta realizan una instruccion asumiendo la maquina actual, 
modificara su sistema con configuraciones no adecuadas o incluso las exportara 
a el paquete que creara y publicara.

Todos los comandos estan dise;ados de forma que puede siempre ejecutarlos asi 
ya hayan realizado su funcion, no afectara volver ejecutarlos.

* crear el usuario `general`, si ya esta creado este comando no afectara.
* configurar el `doas` para que pueda ejecutar cualquier comando con su clave
* agregar el usuario a los grupos para que funcione y permita manejar su entorno

```bash
useradd -m -U -c "" -G abuild,wheel,input,disk,floppy,cdrom,dialout,audio,video,lp,netdev,games,users,ping general

cat >  /etc/doas.d/general.conf << EOF
permit keepenv :general
EOF

for u in $(ls /home); do for g in abuild disk lp floppy audio cdrom dialout video lp netdev games users ping; do addgroup $u $g; done;done
```

El usuario `general` esta incluidos en distintos grupos para poder acceder a 
discos, sonido, coneciones, video, jugar, y realizar operaciones de red. Note 
que **el primer grupo se llama `abuild`** que es creado por cualqueir paquete de 
alpine que implique trabajar con compilacion desde `abuild`. Si el usuario no 
esta agregado a dicho grupo no podra trabajar completamente creando paquetes.

Ahora cierre la sesión de la cuenta de root, e inicie sesión como `general`. 

### Iniciando sesion y configurando el entorno

A partir de aquí todo se debera hacer en una cuenta de usuario `general`, y las 
operaciones que requieren privilegios de superusuario se pueden hacer con el 
prefijo `doas` antes de cualqueir comando que desee que se ejecute. Ojo, solo 
ejecute y prefija con el `doas` cuando sea necesario.

* crear el directorio de trabajo estandar y asignar los permisos adecuados
* configurar el tipo de integracion de cambios de git
* configurar la capacidad de datos que se suben con git
* Crear el directorio donde el proceso de compilacion reusara los archivos descargados
* configurar abuild para que compile maxima optimizado y use las rutas correctas
* Asignar permisos para correcta ejecucion del entorno en las rutas correctas
* configurar la indicacion del usuario empaquetador en el git y el abuild
* generar la llave privada para firmar los paquetes con una firma digital publica

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

Notese que cada apartado de la lista anterior tiene un grupo de comandos que 
estan separados por una linea vacia, significa que no solo explica cada grupo 
y que hace sino que tambien indica que debe esperar que este grupo de comandos 
se hayan ejecutado correctamente antes de "pegar" el siguente lote a ejecutar.

### Ejercicio 1 crear un paquete nuevo con internet

Esto es para cuando el programa no esta aun empaquetado en alpine, este no existe 
y lo piensa empaquetar dede internet para alpine. Ejecute los siguientes 
pasos descritos con sus comandos a continuacion:

* Crear un directorio de trabajo y cambiarse a dicho directorio
* Crear el archivo APKBUILD con la ayuda del sistema de desarrollo abuild
    * El parametro `-f` asegura se creen archivos frescos para alpine empaquetar
    * El parametro `-d` le asigna la descripcion `crea dialogos segun el entorno`
    * El parametro `-n` le coloca el nombre del paquete a `easybashgui`
    * El parametro `-l` la ha asignado la licencia `MIT` (para el APKBUILD solo)
    * Se usa `-a` porque este programa emplea Makefile, asi generara las isntrucciones
    * el ultimo parametro es la orden de descarga directa del programa empaquetar
* Se creara el directorio con el nombre del paquete
    * Internamente tendra el archivo `APKBUILD`
    * Las fuentes del programa estaran descargadas en `/var/cache/distfiles`
    * Despues edite el archivo `APKBUILD` segun las opciones, para que empaquete
    * Se creara unos directorios, donde se realizara la compilacion
    * El archivo `APKBUILD` no siempre detecta bien la url que se le paso de fuentes
* Despues de ajustar se actualiza en checksum del `APKBUILD` para generarlo
* Entonces ya se puede mandar a compilar y crear el paquete APK
    * El resultado estara en el directorio `/home/general/Devel/packages/`

```bash
mkdir -p /home/general/Devel/alpine-apkbuilds/main

cd /home/general/Devel/alpine-apkbuilds/main

newapkbuild -f -d "crea dialogos segun el entorno" -n easybashgui -l MIT -a https://github.com/BashGui/easybashgui/archive/refs/tags/13.0.0.tar.gz

cd /home/general/Devel/alpine-apkbuilds/main/easybashgui
echo "aqui editar los archivos presentes y salvar"

abuild checksum

abuild -r
```

### Ejercicio 2 crear un paquete nuevo con las fuentes local

Esto es para cuando el programa no esta aun empaquetado en alpine, este no existe 
y usted lo tiene local o quiere la fuentes localmente. Ejecute los siguientes 
pasos descritos con sus comandos a continuacion:

* Crear un directorio de trabajo y cambiarse a dicho directorio
* Crear el archivo APKBUILD con la ayuda del sistema de desarrollo abuild
    * El parametro `-f` asegura se creen archivos frescos para alpine empaquetar
    * El parametro `-d` le asigna la descripcion `crea dialogos segun el entorno`
    * El parametro `-n` le coloca el nombre del paquete a `easybashgui`
    * El parametro `-l` la ha asignado la licencia `MIT` (para el APKBUILD solo)
    * Se usa `-a` porque este programa emplea Makefile, asi generara las isntrucciones
    * el ultimo parametro es la orden de descarga directa del programa empaquetar
* Se creara el directorio con el nombre del paquete
    * Internamente tendra el archivo `APKBUILD`
    * Descargar manualmente las fuentes en el directorio recien creado
    * Despues edite el archivo `APKBUILD` segun las opciones, para que empaquete
    * El archivo `APKBUILD` debe decirle que la fuente esta all mismo sin http 
* Despues de ajustar se actualiza en checksum del `APKBUILD` para generarlo
* Entonces ya se puede mandar a compilar y crear el paquete APK
    * El resultado estara en el directorio `/home/general/Devel/packages/`

```bash
mkdir -p /home/general/Devel/alpine-apkbuilds/main

cd /home/general/Devel/alpine-apkbuilds/main

newapkbuild -f -d "crea dialogos segun el entorno" -n easybashgui -l MIT -c easybashgui-13.0.0

cd /home/general/Devel/alpine-apkbuilds/main/easybashgui
aria2c -o easybashgui-13.0.0.tar.gz https://github.com/BashGui/easybashgui/archive/refs/tags/13.0.0.tar.gz
echo "aqui editar los archivos presentes y salvar"

abuild checksum

abuild -r
```

### Generacion automatica de mi repositorio

Despues de crear su paquete nuevo existosamente si se compilo, el nuevo binario 
estara en `/home/general/Devel/packages/main`, pero ojo se creara un directorio 
por cada arquitectura, siendo estos `x86`, `x86_64`, `armv7` etc

El archivo indice `APKINDEX.tar.gz` es actualizado y firmado automatico, 
y este repositorio puede ser expuesto via web con apache o lighttp, pero 
no directamente el directorio, sino empleando aliasing:

Caso apache2:

```
Alias /packages /home/general/Devel/packages/
<Directory /home/general/Devel/packages/>
    Require all granted
</Directory>
```

Caso lighttpd

```
alias.url += (
     "/packages/"	    =>    "/home/general/Devel/packages/"
)
$HTTP["url"] =~ "^/packages/" {
    dir-listing.activate = "enable"
}
EOF
```

## Anexos 

### Como se creaban los APKBUILD

En sistemas Alpine mas antiguos, `abuild -c -n easibashgui` era la forma de 
crear archivos `APKBUILD`. El `easybashgui` era un parámetro de la opción `-n`, 
por lo que el orden de `-c` y `-n` importaba.

### sobre abuild-keygen

Como anecdota, el comando `abuild-keygen` es una evolucion que en antiguas 
versiones habria que hacer a mano asi:

```
openssl genrsa -out /home/general/.abuild/general@venenux.xxx.key.rsa 2048
openssl rsa -in /home/general/Devel/general@venenux.xxx.key.rsa -pubout -out /etc/apk/keys/general@venenux.xxx.key.pub
cat > /etc/abuild.conf << EOF
PACKAGER_PRIVKEY="/home/general/.abuild/general@venenux.xxx.key.rsa"
EOF
```

### Generacion manual de repositorio

Puede que el paquete ya existe solo en binario, habra que agregar a mano, por 
ende debe crear la rama y la arquitecturan dentro de el directorio de paquetes.

Nuestro repositorio ha sido configurado en `/home/general/Devel/packages/main`, 
pero ojo se creara un directorio por cada arquitectura, siendo estos `x86`, 
`x86_64`, `armv7` etc, dentro de este.

Despues colocar el paquete dentro del directorio de la arquitectura, si el 
paquete es un script o no depende de arquitectura, colocarlo en todos los 
directorios de arquitectura.

Finalmente se crea el indice sin firma y despues se firma manualmente.

* se crea un directorio de paquetes y dentro uno para cada arquitectura
* se descarga un binario, aqui uno ficticio
* se crea y agrega el mismo en el repositorio de paquetes en su arquitectura
* se genera el indice de paquetes listando los APKs del directorio
* se copia la llave sin firmar para que ofrezca los indices

```
mkdir -p /home/general/Devel/packages/main/x86

cd /home/general/Devel/packages/main/x86

wget https://www.google.com/alpinepackage-1-1.apk

apk index -o /home/general/Devel/packages/main/x86/APKINDEX.unsigned.tar.gz /home/general/Devel/packages/main/x86/*.apk

cp /home/general/Devel/packages/main/x86/APKINDEX.unsigned.tar.gz /home/general/Devel/packages/main/x86/APKINDEX.tar.gz
```

Estos paquetes deberan ser instalado o indicados usando `--allow-untrusted` en 
el comando `apk`, tanto para la accion `update` asi como para instalar.

Para firma el repositorio debe haber ejecutado todos los pasos de la configuracion 
mencionados en la seccion 2 de este documento, y despues de tener las llaves 
tanto publica como la privada poder firmarlos asi:

```
abuild-sign -k /home/general/.abuild/general@venenux.xxx-5629d7e6.rsa /home/general/Devel/packages/main/x86/APKINDEX.tar.gz
```

# mas informacion

- Author
  - Sodomon as https://t.me/alpine_linux_espanol/26413
- 🗯 IRC
  - 💬 `##alpine_telegram_english` (roto)
- 📱 Telegram https://t.me/alpine_linux
  - 🇨🇴 https://t.me/alpine_linux_espanol
  - 📡 https://t.me/latam_programadores
- Matrix
  - 👥 https://matrix.to/#/#alpine-linux-espanol:matrix.org

# LICENSE

**CC BY-NC-SA** Sodomon 2020
