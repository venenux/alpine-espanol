
De cualquier manera, esta sección describe, paso a paso, cómo obtener e instalar el uso 
de las **principales utilidades de configuración**, así como explicaciones para una instalación 
**mucho mas completa (los de alpine minimalizan tanto que eso ni sirve) en una computadora común 
basada en UEFI exclusivamente, especialmente la DELL OPTIPLEX 7071, el cargador Grub admite la 
indexación de particiones base EFI y GPT. Se uso un DELL OPTIPLEX 7071 i9 9900K 10 gen para est documento.

# 0. Source media

Primero descargue un medio de origen, desde https://alpinelinux.org/downloads/ , todas 
las imágenes necesitarán una conexión a Internet, excepto "extended" que vienen con paquetes 
de necesidad mínima, pero solo están basados ��en x86.

Para todos los usuarios, no les importa qué sistema operativo viene o qué arquitectura, recomendamos 
usar `balena-etcher-electron` https://www.balena.io/etcher/ para flashear una unidad USB desde 
cualquier sistema, por supuesto, debe ejecutarlo como usuario root o administrador.

* descargar el archivo iso de la imagen multimedia
* descargar el programa balena-etcher-electron
* ejecute el programa balena-etcher-electron como root en la sesión X o usando `su -c`
* haga clic en el icono de "seleccionar imagen" y abra el archivo iso de imagen descargado
* Conecte la unidad USB a la computadora y se mostrará automáticamente.
* ¡Entonces solo desmonte pero no expulse el dispositivo! ¡Asegúrese de hacer una copia de seguridad de todos los archivos en la unidad USB!
* después de que el programa balena-etcher-electron muestre la unidad USB como "sdb" por ejemplo, haga clic en flash
* Espere un momento y cuando termine, cierre y retire el USB para conectarlo a la computadora de destino de instalación.

# 1. Installation

Después de actualizar el medio USB o grabar un CDROM, instale el medio en el compartimiento 
de unidad respectivo de la computadora y encienda la computadora. En los DELL OPTIPLEX no debe usar 
los puertos USB 3.0 o los que son azules, use los que estan al lado de el plug de red cableada o 
el primero izquierdo frontal.

Seleccione el medio de inicio adecuado, en DELL siempre es la F12 y presione en la pantalla de inicio "Dell" 
y cuando aparezca el menú, seleccione el medio adecuado EN EL MODO UEFI, es decir el usb se mostrara en dos 
secciones al mismo y debe seleccionar el de la seccion UEFI porque si seleciona en modo legacy no se instalar el 
manejador de arranque en el registro y dependera de otro manejador de arranque desde el UEFI.

## 1.1. Installation from scrat

La instalación se realizó sin problemas usando 3.11.3 pero para este modelo de PC con UEFI necesitamos minimo
el alpine 3.15.0 y se recomeinda ya para los equipos desde 2020 que sea alpine 3.18 o 3.16

En este se uso y se recomiendo para OPTIPLEX 7070/7071 midtower la vesion 3.18+ ya que lso discos son SSD PCIe de mas 
de 500GB y este tutorial asume que se usara todo el disco:

```
setup-keymap us us-altgr-intl

export BOOT_SIZE=500
export SWAP_SIZE=4096
export BOOTLOADER=grub

setup-alpine
```

Esta secuencia debe escribirla tal cual y configurara el disco en esta manera:

* `/dev/sda1` como BOOT en 500Mb en `/boot`
* `/dev/sda2` como SWAP en 4Gb
* `/dev/sda3` como ROOT en 470Gb en `/`

Cabe destacar que se usa la imagen alpine extended por lo que en la pregunta de red obviar cualquier configuracion 
de inalambrica o estatica si no sabe lo que hace o no es experto en redes.

> Warning; debe de crear el usuario `general` si la instalacion se lo pide de nombre `general` no cambie esto.

## 1.2  Installation and then upgrade

Si instalaste una version 3.14 o 3.15 despues puedes actualizar a 3.18 asi:

```
sed -s -i -r 's|v$(cat /etc/alpine-release | cut -d'.' -f1,2)|v3.18|g' /etc/apk/repositories

apk update

apk upgrade -a
```

OJO no ahondaremos en la configuacion de red, arvertimos que use la cableada y use DHCP, facilite el trabajo, 
la configuracion de red aunque todos los dispositivos de red hoy se soportan, los DELL OPTIPLEX no presentan problemas 
con la red cableada pero si puede que de problemas si le instala un dispositivo PCIe WIFI+Bluetooh lo cual 
no recomendamos use WIFI sino CABLE para este tutorial, para otros metodos con el internet consulte aqui las guias.

## 1.3 Post install setup

Necesitamos paquetes y configuraciones mínimos importantes para la consola y el desarrollo, 
también algunas configuraciones específicas para la conexión remota desde otros Linux, 
debido a que hoy Alpine no tiene un buen software x11 remoto que x11vnc.

Lo más recomendado es tener un usuario de acceso llamado `remoto` y un usuario de uso general 
normal llamado `general` por conveniencia. En los siguientes comandos configuraremos un entorno 
limitado muy reforzado para cualquier usuario nuevo y crearemos esos dos usuarios, no importando 
si ya lo hizo o no en la instalacion esto comandos contenplan eso:

```
mkdir -p /etc/skel/

cat > /etc/skel/.logout << EOF
history -c
/bin/rm -f /opt/remote/.mysql_history
/bin/rm -f /opt/remote/.history
/bin/rm -f /opt/remote/.bash_history
EOF

cat > /etc/skel/.cshrc << EOF
set autologout = 30
set prompt = "$ "
set history = 0
set ignoreeof
EOF

cp /etc/skel/.cshrc /etc/skel/.profile

adduser -D --home /opt/remote --shell /bin/ash remote

echo "secret_new_remote_user_password" | chpasswd

adduser -D --shell /bin/bash general

echo "secret_new_general_user_password" | chpasswd
```

Despues de esto debemso configurar el repositorio, para eso el internet es primordial, 
lamentablemente casi todo hoy es con internet, hay maneras de traerse todo local pero 
no lo asumiremos aqui:

```
cat > /etc/network/interfaces << EOF
auto lo
iface lo inet loopback
EOF

setup-interfaces -a

echo -e "\n" | setup-dns 8.8.8.8

rc-service networking restart
```

Ahora agregamos repositorios de donde sacaremso los paquetes principales del sistema operativo Alpine 
(no todo el software viene de alpine pero el principal si):

```
cat > /etc/apk/repositories << EOF
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```

Todo esto lo necesitamos para que la instalaicon de alpine la cual es minimalista se comporte como 
cualquier otra isntalacion basura de linux lleno de muchos comandos faciles pero de relleno.

Ahora configuramos el entorno para que los usuarios y lso comandos se comporten correctamente 
como indican los manuales publicados afuera:

```
cat > /root/.cshrc << EOF
unsetenv DISPLAY || true
HISTCONTROL=ignoreboth
EOF

cp -f /root/.cshrc /root/.profile

apk del dropbear

apk add openssh openssh-doc readline readline-doc

apk add man man-pages lsof lsof-doc less less-doc nano nano-doc curl curl-doc

apk add attr dialog dialog-doc bash bash-doc bash-completion grep grep-doc

apk add util-linux pciutils usbutils binutils findutils readline

apk add util-linux-doc pciutils-doc usbutils-doc binutils-doc findutils-doc

export PAGER=less
```

Ahora en este punto tenemos lista una consola Linux (sin sesión gráfica de escritorio), se puede 
acceder a cada consola presionando al mismo tiempo las teclas `Ctrl`+ `Alt`+ `FX` donde `X` pueden 
ser del 1 al 6 como ya conocemos como teclas de función, por ejemplo, si presionamos `Ctrl+Alt+F3` 
veremos una consola TTY3 con el mensaje "iniciar sesión"; donde puede ingresar el nombre de usuario 
y luego, después de presionar ingresar, ingresar la contraseña del usuario, si ambos están funcionando, 
se mostrará un mensaje de consola de Linux con el simbolo `$` listo para recibir entradas de comandos.

# que hacer despeus de instalar:

Lease [Receta despues de instalar: configuracion y paquetes](../recetas/alpine-recetas-configuracion-y-paquetes-sistema.md).
