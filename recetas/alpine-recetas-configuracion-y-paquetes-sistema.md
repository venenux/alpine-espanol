Los paquetes proporcionan los componentes básicos de un sistema operativo, 
junto con librerias, aplicaciones, servicios y documentación compartidos.

Este documento guiará a los nuevos usuarios a las necesidades básicas sobre:

1. instalar paquetes (software para instalar) y ..
2. configuraciones mínimas iniciadas (configuraciones del sistema).

Después de instalar Alpine, todos los sistemas Alpine son "busybox basados" 
lo que significa que es un sistema "minimo" o recortado, con esta guia 
se podra colocar igual que si de un linux comun se tratase en la consola.

Una vez realizado esto en este documento se puede configurar 
[los graficos y escritorio de trabajo con el documento alpine-recetas-configuracion-entorno-grafico-y-escritorio.md](alpine-recetas-configuracion-entorno-grafico-y-escritorio.md)

## Nuevos usuarios: nombre, red y root

#### nombre de la máquina

El nombre de su máquina, `hostname` host es un nombre 
legible por humanos de su computadora, que ya se usa en la red:

```
hostname MyMachine

echo "MyMachine"> / etc / hostname

cat> / etc / hosts << EOF 
127.0.0.1 MyMachine.domain.x mymachine localhost.localdomain localhost 
:: 1 localhost localhost.localdomain 
EOF
```

#### configuración de la red

Solo hay tres tipos de conexiones a Internet, con cable (fijo por cable), 
wifi (inalámbrico) y pptp (módems), esta sección se centrará solo en el cableado, 
ya que la configuracion wifi requeire de mayores elaborados pasos.

Debe conectar el cable de red a el interfaz de red del computador, 
despues realizar estos comandos:

```
cat> / etc / network / interfaces << EOF
auto lo
iface lo inet loopback
EOF

setup-interfaces -a

echo -e "\ n" | setup-dns 8.8.8.8

rc-service networking restart
```

#### configuracion de "root"

El usuario `root` es el dios del sistema Linux Alpine, ejecuta solo las 
tareas más profundas y no debe usarse como usuario normal 
ni para tareas comunes. Para usuarios comunes, use la ultima seccion.

Cuando Alpine Linux se instala por primera vez de manera predeterminada, 
viene con el usuario `root` sin contraseña establecida, por lo que el 
primer paso después del arranque en la instalación alpine es la clave de root:

```
cat > /root/.cshrc << EOF
unsetenv DISPLAY || true
HISTCONTROL=ignoreboth
EOF

cp /root/.cshrc /root/.profile

echo "secret_new_root_password" | chpasswd
```

Por supuesto, debe cambiar "secret_new_root_password" por una contraseña 
adecuada proporcionada por usted.

## Nuevos usuarios: paquetes de software de sistema

Los paquetes y programas en alpine dependen del origen y finalidad, la 
gran mayoria no se usan directamente y sonlos que vamos instalar.

**NOTA**: requerira internet, si usa wifi consulte [alpine-recetas-redes-y-conexciones.md](alpine-recetas-redes-y-conexciones.md).

#### habilitar repositorio

Los depósitos de paquetes o repositorios es de donde se obtienen programas
del sistema, para despues obtener programas desde fuentes externas, ejecute:

```
cat > /etc/apk/repositories << EOF
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
#http://dl-cdn.alpinelinux.org/alpine/edge/testing
EOF

apk update
```

#### instalar herramientas basicas

Para una similitud mínima a otros Linux ejecutar:

```
apk add linux-firmware

apk add htop htop-doc links links-doc readline readline-doc

apk add lsof lsof-doc less less-doc nano nano-doc curl curl-doc

apk add attr dialog dialog-doc bash bash-doc bash-completion grep grep-doc

apk add util-linux pciutils usbutils binutils findutils readline
apk add util-linux-doc pciutils-doc usbutils-doc binutils-doc findutils-doc

export PAGER=less

apk add python2 python3 python2-doc python3-doc

apk add pcre pcre-doc npth gmp gmp-doc

apk add testdisk ntfs-3g ntfs-3g-doc btrfs-progs-extra btrfs-progs-doc e2fsprogs-extra e2fsprogs-doc dosfstools dosfstools-doc

apk add imagemagick xorriso rsync antiword asciidoc wv wv-doc

apk add aspell aspell-en aspell-ru aspell-de aspell-utils aspell-doc aspell-libs

apk add aria2 wget wget-doc elinks elinks-doc macchanger macchanger-doc 

apk add cpufrequtils cpufrequtils-doc
rc-service cpufrequtils start
rc-update add cpufrequtils

apk add tar zlib xz zip unzip p7zip cpio lha sharutils lzip lzo gzip rpm cabextract
apk add tar-doc zlib-doc xz-doc zip-doc p7zip-doc cpio-doc lha-doc sharutils-doc gzip-doc rpm-doc
```

#### páginas de manual en alpino

Notese que en la seccion anterior algunos paquetes tienen una relación "xxx-doc", 
si desea una página de manual de cualquier programa en un paquete de software, 

Para habilitar las paginas de manual ejecute:

```
apk add man man-pages
```

Siempre debe instalar el paquete relacionado `<programa>` y también `<program>-doc`, 
pero a veces no todos los paquetes tienen un paquete de documentación disponible.

#### coreutils libc y utmps en alpine

No todo el sistema coreutils esta soportado debido a `muslc` pero se 
tiene equivalentes desde el repositorio `testing`.

El `coreutils` esta embebido en `busybox` hay dos opciones para instalarlo:
* el original `coreutils` desde el principal.
* el optimizado `UBase` desde testing edge.

Es recomendado el original para que asi no difieran los comandos de otros programas:

```
cat > /etc/apk/repositories << EOF
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://dl-cdn.alpinelinux.org/alpine/edge/testing
EOF

apk update

apk add utmps 

cat > /etc/apk/repositories << EOF
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
#http://dl-cdn.alpinelinux.org/alpine/edge/testing
EOF

apk update

apk add coreutils coreutils-doc
```


#### paquetes apk para base de sonido

Actualmente, los paquetes alsa son mínimos para el sonido Linux Alpine, 
en las computadoras de escritorio existen los sistemas gstreamer, phonon y pulseaudio, 
pero dichas configuraciones son mas propias del documento para graficos y escritorio.

```
apk add alsa-utils alsa-utils-doc alsa-lib alsaconf

rc-service alsa start 

rc-update add alsa
```

**IMPORTANTE**: Las máquinas modernas tienen doble GPU y tarjetas de sonido duales, 
las segundas son siempre como primarias y son una tarjeta de sonido de clase HDMI, 
comúnmente se detectan mal y deben ser configuradas para que se use la correcta.

## Nuevos usuarios: gestión de usuarios e inicios de sesion.

Solo root puede administrar usuarios . Crear una cuenta le permite tener 
su propio directorio o `$HOME` de archivos y le permite limitar el acceso 
a la configuración del sistema operativo por razones de seguridad.

#### Creación de usuarios y valores predeterminados

Lo más recomendable es tener un usuario de acceso aquí 
llamado "remoto" y un usuario de uso general normal aquí llamado "general" 
por conveniencia, en los siguientes comandos configuraremos un entorno 
limitado muy resistente para cualquier usuario nuevo:

Los siguientes comandos primero configurarán el inicio de sesión 
del entorno raíz y luego asignarán una nueva contraseña a cada usuario creado:

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

**NOTA**: "general" y "remoto" son los nombres de los usuarios, DEBEN estar 
en minúsculas, sin espacios sin símbolos

Tenga en cuenta que esos usuarios se crean con una configuración mínima. 
Por supuesto, debe cambiar "secret_new_remote_user_password" por una clave 
adecuada, también igual que "secret_new_general_user_password" por una clave.

#### Usuarios y permisos de uso

Se emplea los grupos para acceder a los dispositivos o realizar conexiones, 
por lo que esos son los grupos recomendados donde el usuario debe tener 
y el comando que se ejecuta pra establecerlos en los usuarios creados:

```
for u in $(ls /home); do for g in disk lp floppy audio cdrom dialout video netdev games users; do addgroup $u $g; done;done
```

Estos grupos minimos se explica su empleo para el usuario comun aqui:

* **disk** solo si queire que el usuario pueda acceder a particiones extras
* **lp**  para utilizar servicios de impresión y gestión de impresoras
* **floppy** grupo compatible acceso a dispositivos especiales externos
* **audio** de escuchar audio y gestionar volúmenes de sonido como usuario
* **cdrom** usar y ver el contenido de un DVD, BR o CD al insertarlo
* **dialout** marcar conexiones privadas y uso de módems por un usuario
* **tape** si planea usar dispositivos especiales para respaldo
* **video** uso de cámaras, capturadoras, scaners, GPU avanzadas
* **netdev** para gestión de conexiones de red como usuario normal
* **kvm** Solo si, el usuario administrará gráficamente máquinas virtuales
* **games** si se emplea juegos y se comparten puntajes y marcas
* **cdrw** grabar en discos RW-DVD, RW-BR o RW-CD en un dispositivo
* **apache** necesario para desarrollo web como usuario normal
* **usb** acceder a dispositivos usb y poder ver su contenido
* **users** si se usa archivos comunes, necesario para escritorio grafico

#### Usuarios y gestion

La administración de usuarios se puede hacer con el busybox si mas paquetes, 
pero en el repositorio testing esta un programa para modificar y administrar:


```
cat > /etc/apk/repositories << EOF
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://dl-cdn.alpinelinux.org/alpine/edge/testing
EOF

apk update

apk add libuser

cat > /etc/apk/repositories << EOF
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
#http://dl-cdn.alpinelinux.org/alpine/edge/testing
EOF

apk update

touch /etc/login.defs

touch /etc/default/useradd
```

El programa `libuser` permite la administracion de la sesion personal, 
o por el administrador del sistema, ejemplo de su uso segun el caso:

* Si el usuario, para cambiar su shell: inicie sesión como ese usuario y 
luego dentro de su sesión de terminal ejecute `lchsh`
* Si el administrador para cambiar de un usuario diferente, ejecútelo 
como administrador o como root asi: `lchsh general`

Donde "general" era el nombre de un inicio de sesión de usuario creado 
en la seccion anterior.

#### continuar al escritorio

Antes de continuar, debe haber hecho los paso descritos anteriormente 
en este documento. 

Si todo lo aqui fue realizado siga con [alpine-recetas-configuracion-entorno-grafico-y-escritorio.md](alpine-recetas-configuracion-entorno-grafico-y-escritorio.md)

# Vea tambien:

* [README informacion general](../README.md)
* [alpine-recetas-configuracion-entorno-grafico-y-escritorio.md](alpine-recetas-configuracion-entorno-grafico-y-escritorio.md)
* [Instalacion de programas](../programas/programas-esenciales-todo-en-uno.md)
* [Informes y tablas de compatibilidad](../informes/hardware-y-versiones-alpine-recomendados.md)
