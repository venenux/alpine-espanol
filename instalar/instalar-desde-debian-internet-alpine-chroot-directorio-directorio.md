
Este documento guiara para **instalar Alpine desde Debian linux ya presente**, 
en este caso desde Debian o similares (que usan apt o archivos paquetes deb)
Esta variante asume tiene internet, esta en trabajo otro documento para otras vias.

Debido a que no tenemos usb ni imagen alpine por red, ni cdrom, usaremos Internet, 
en dado caso si desea usar disco real o CD/USB y no usar `chroot` (desde el Debian ejecutar Alpine) 
lease: [instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md](instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md)

## preparar medios instalar

Todo lo aqui explicado se detallara mas adelante en el documento.
Se necesita un "medio de origen" (el alpine linux) y un "medio destino" donde instalar.

Prepare el **medio destino** sea una particion o simplemente un directorio, sea cual sea el destino, 
este se debera montar (es decir `mount`) o estar en el directorio `/alpine` para este documento.


1. Opcion directa un directorio sin particion externa ni otro disco externo, directo 
en un directorio del mismo linux presente, simplemente crear el directorio `mkdir /alpine` para usarlo.
2. Opcion disco externo (asumiendo el segundo disco es `/dev/sdb`) montar la particion 
en el directorio (asumiendo es `/dev/sda1`) como `mkdir /alpine;mount /dev/sda1 /alpine` para usarlo.
3. Opcion usando un disco virtual es posible si `vdfuse` o `qemu-tools` y monte este disco 
virtual y la particion en dicho directorio, favor usar el documento especifico.

**IMPORTANTE** sea cual sea el medio destino, este debe tener reservado espacio de 1000M como minimo.

Prepare el **medio origen** Una vez listo el "medio destino", descargando la herramienta 
`wget http://mirror.yandex.ru/mirrors/alpine/latest-stable/main/x86/apk-tools-static-2.10.4-r2.apk -O /var/tmp/alpine/apk-tools-static-2.10.4-r2.apk` 
este se encargara de realizar la instalacion base del sistema con el internet. Recuerde que 
si no hay red necesitara un medio que lo emule, refierase a la documentacion especifica que descarga el repo completo.

**IMPORTANTE** la version 3.10 de alpine remueve mucha paqueteria que quizas necesite, como mayor ejemplo QT4 no esta mas.

#### Requisitos

* Internet disponible para el **medio origen** y los paquetes de alpine.
* Linux instalado para el **medio destino**, Debian en este caso minimo 5 lenny con kernel 2.6.26 minimo
* Paquetes de raiz de sistema `chroot` (`apt-get install coreutils`)
* Paquetes soporte para binarios `binfmt-support` (`apt-get install binfmt-support`)
* Paquetes base `wget`, `sed`, `tar`, `gzip`, `bzip2` (`apt-get install wget bzip2 tar sed gzip less`)

## Instalacion alpine base en medio destino chroot

#### preparacion de medio destino y programas requeridos


```
apt-get install wget sed tar gzip bzip2 binfmt-support coreutils

update-binfmts --enable

mkdir /alpine

mkdir /var/tmp/alpine

wget http://mirror.yandex.ru/mirrors/alpine/latest-stable/main/x86/apk-tools-static-2.10.4-r2.apk \
 -O /var/tmp/alpine/apk-tools-static-2.10.4-r2.apk

cd /var/tmp/alpine && tar -xzf apk-tools-static-*.apk
```

#### instalacion del medio origen en medio destino


```
/var/tmp/alpine/sbin/apk.static \
 -X http://mirror.yandex.ru/mirrors/alpine/latest-stable/main \
 -U --allow-untrusted --root /alpine --initdb add alpine-base
```

#### Configuracion del chroot medio destino

```
echo -e 'nameserver 8.8.8.8\nnameserver 2620:0:ccc::2' > /alpine/etc/resolv.conf
cp /etc/resolv.conf /alpine/etc/
mkdir -p /alpine/root

mkdir -p /alpine/etc/apk
echo "http://mirror.yandex.ru/mirrors/alpine/latest-stable/main" > /alpine/etc/apk/repositories

```

Crearemos los nodos a mano dado los kernel asi como de sistemas de archivos 
casi siempre seran distintos.. asegurando independencia en el chroot.

**ADVERTENCIA** se crean nodos para sda y sdb pero dentro del chroot sda es alpine afuera no

```
mknod -m 666 /alpine/dev/full c 1 7
mknod -m 666 /alpine/dev/ptmx c 5 2
mknod -m 644 /alpine/dev/random c 1 8
mknod -m 644 /alpine/dev/urandom c 1 9
mknod -m 666 /alpine/dev/zero c 1 5
mknod -m 666 /alpine/dev/tty c 5 0
mknod -m 666 /alpine/dev/sda b 8 0
mknod -m 666 /alpine/dev/sda1 b 8 1
mknod -m 666 /alpine/dev/sda2 b 8 2
mknod -m 666 /alpine/dev/sda3 b 8 3
mknod -m 666 /alpine/dev/sda4 b 8 4
mknod -m 666 /alpine/dev/sdb b 8 16
mknod -m 666 /alpine/dev/sdb1 b 8 17
mknod -m 666 /alpine/dev/sdb2 b 8 18
mknod -m 666 /alpine/dev/sdb3 b 8 19
mknod -m 666 /alpine/dev/sdb4 b 8 20

mount -t proc none /alpine/proc
mount -o bind /sys /alpine/sys

```

## Usando el alpine del medio destino chroot

#### Ejecutar chroot en el sistema alpine o anfitrion

Todos los comandos anteriores fueron realizados en el sistema "host" o "anfitrion" , 
el sistema desde donde se monta es el "anfitrion" que ejecutara el chroot al alpine:

```
chroot /alpine /bin/sh -l
```

Una vez ejecutado chroot ahora todos los comandos es en el sistema "guest" o "huesped" , 
el sistema alpine esta en el "medio destino" y este conjunto es el "huesped" alpine:

#### Usando el alpine despues de ejecutar chroot

Ahora como se ejecuto la orden chroot el sistema es alpine y todos los comandos 
son de alpine, se puede incluso ejecutar este en otro "xorg" en otra pantalla , 
los primeros comandos son de herramientas necesarias.. 

```
cat > /etc/apk/repositories << EOF
http://mirror.yandex.ru/mirrors/alpine/latest-stable/main
http://mirror.yandex.ru/mirrors/alpine/latest-stable/community
EOF

apk update

apk add attr dialog dialog-doc bash bash-doc bash-completion coreutils coreutils-doc grep grep-doc
apk add util-linux util-linux-doc pciutils usbutils binutils findutils readline
apk add man man-pages lsof lsof-doc less less-doc nano nano-doc curl curl-doc
export PAGER=less

apk add python2 python3

apk add htop testdisk btrfs-progs-extra btrfs-progs-doc e2fsprogs-extra e2fsprogs-doc dosfstools dosfstools-doc
apk add imagemagick xorriso rsync antiword asciidoc wv wv-doc
apk add aspell aspell-en aspell-ru aspell-de aspell-utils aspell-doc aspell-libs

```

Con estos ultimos comandos de apk el sistema esta listo par las proximas tareas, 
notara que este sistema base no tiene mas de 500 a 900 megas de ocupado.

# Vea tambien

* [README varios tipos de instalacion](README.md)
* [Receta despues de instalar: configuracion y paquetes](../recetas/alpine-recetas-configuracion-y-paquetes-sistema.md)
* [instalar-desde-imagen-a-virtualbox-alpinesolo-computadora.md](instalar-desde-imagen-a-virtualbox-alpinesolo-computadora.md)
* [instalar-desde-usb-a-discoreal-alpinesolo-computadora.md](instalar-desde-usb-a-discoreal-alpinesolo-computadora.md) 
* [instalar-desde-cdrom-a-discoreal-alpinesolo-computadora.md](instalar-desde-cdrom-a-discoreal-alpinesolo-computadora.md).
* [instalar-desde-virtualbox-a-discoreal-dualboot-guia.md](instalar-desde-virtualbox-a-discoreal-dualboot-guia.md)
* [instalar-desde-virtualbox-a-discoreal-alpinesolo-guia.md](instalar-desde-virtualbox-a-discoreal-alpinesolo-guia.md)
