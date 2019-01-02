
Esto es escritorio, no servidor, en escritorio se necesita de todo, la seguridad es solo 
en dos lugares, el naveador y el ssh (ya que no montaremos ningun sistema web como servicio mas que desarrollar).


# habilitar repositorios
----------------------------------------

`nano /etc/apk/repositories`

descomentar el de comunity de la version especifica, no descomentar egde.

**NOTA** versiones menores de Alpine a 3.2 no tiene repositorio comunidad.

```
apk update
```


# Sistema base necesario
-------------------------------------------

## programas base linux

```
apk add linux-firmware

apk add bash bash-doc bash-completion coreutils coreutils-doc grep grep-doc
apk add util-linux util-linux-doc pciutils usbutils binutils findutils readline
apk add man man-pages lsof lsof-doc less less-doc nano nano-doc curl curl-doc
export PAGER=less

apk add python2 python3

apk add htop testdisk
apk add imagemagick xorriso rsync
```

## decompresion y compresion

Para interfaz grafica de compresion y decompresion ir a la seccion de escritorio.

```
apk add tar zlib xz zip unzip p7zip cpio lha sharutils lzip lzo gzip
apk add tar-doc zlib-doc xz-doc zip-doc p7zip-doc cpio-doc lha-doc sharutils-doc gzip-doc
```

## comunicacion e internet consola

```
apk add aria2 irssi irssi-doc irssi-proxy fish elinks elinks-doc
```

# gestion remota

Esta por consola y grafica:

## Gestion remota consola

```
apk add openssh
sed "s|#UseDNS.*|UseDNS no|g" -i /etc/ssh/sshd_config
sed "s|#ListenAddress 0.*|ListenAddress 0.0.0.0|g" -i /etc/ssh/sshd_config
sed "s|#PermitRootLogin.*|PermitRootLogin Yes|g" -i /etc/ssh/sshd_config
rc-update add sshd
/etc/init.d/sshd start
```

Por defecto no deja entrar con root, asi que use la seccion crear usuario.

## Gestion remota grafica

```
apk add gtkglext pango atk cairo gdk-pixbuf mesa mesa-gl glib libxcb libxrandr libxv libxxf86vm libxxf86misc libxmu

export alparch=`uname -a | cut -d' '  -f 12`
wget "https://download.anydesk.com/linux/anydesk-4.0.1-$alparch.tar.gz" -O anydesk-4.0.1-$alparch.tar.gz

```

(WIP) nomachine


# Entorno grafico
---------------------------------

Para tener el entorno grafico y los programas graficos 
(mal llamados, pues esto es en general el escritorio grafico)
necesitamos el subsistema grafico, soporte 3d y configuracion de placa video:


## Subsistema grafico


```
apk add eudev udisks2 udisks2-doc
apk add xorg-server xorg-server-xephyr xf86-video-vesa xf86-video-intel xf86-input-evdev  xinit
apk add xf86-input-mouse xf86-input-synaptics xf86-input-keyboard
```

## Placas de video

1). Debemos agregar soporte 3d asi sea falso, hoy dia la estupida dependencia lo exige:

```
apk add glib libxcb libxrandr libxv libxxf86vm libxxf86misc libxmu xcb-util-cursor xcb-util-image xcb-util-keysyms
apk add mesa mesa-gl mesa-egl mesa-gles mesa-gbm mesa-glapi mesa-osmesa xf86-video-modesetting
```

**NOTA** no instalar kbd si tiene laptop a menos que sepa lo que hace

2). Despues segun al tarjeta/placa de video OJO mo mexclar los "mesa-dri-xxx":

* Intel:

```
apk add xf86-video-intel mesa-dri-intel mesa-vulkan-intel
```

* AMD/ati:

```
apk add xf86-video-r128 xf86-video-ati mesa-dri-ati mesa-vulkan-ati
```

* Nvidia:

```
apk del xf86-video-nv
apk add xf86-video-nouveau mesa-dri-nouveau
```

* Otras:

```
apk del mesa-dri-nouveau
apk del mesa-dri-ati
apk del mesa-dri-intel
apk xf86-video-amdgpu xf86-video-openchrome xf86-video-s3 xf86-video-savage xf86-video-sis mesa-dri-swrast 
```

para las voodoo usar `xf86-video-tdfx` pero no tendra 3d porque glidelibs no compila en alpine. (WIP)


3). Y listo se ejecuta si el script correspondiente:

`setup-xorg-base`


# Escritorios graficos
--------------------------


## Paquetes base graficos

```
apk add gtkglext pango atk cairo gdk-pixbuf gtk+2.0 gtk+3.0 gtk-engines gtk-engines-industrial
apk add gtkmm atkmm pangomm cairomm gtkmm3 hicolor-icon-theme imagemagick-c++ imagemagick-libs ghostscript
apk add shared-mime-info
apk add ttf-dejavu ttf-droid ttf-freefont ttf-liberation ttf-linux-libertine ttf-ubuntu-font-family
```

## Sonido y multimedia

```
apk add alsa-utils alsa-utils-doc alsa-lib alsaconf
rc-service alsa start
rc-update add alsa
apk add gstreamer0.10 gstreamer0.10-tools gstreamer0.10-docs gst-plugins-base0.10 gst-plugins-base0.10-doc
apk add gstreamer gstreamer-tools gstreamer-doc gst-plugins-base gst-plugins-base-doc

```

## Escritorio LXDE y openbox

```
apk add dbus dbus-x11 dbus-glib dhcpcd-dbus dbus-libs dbus-doc dbus-glib-doc
rc-service dbus start
rc-update add dbus
apk add openbox pcmanfm lxsession ghostscript
```


apk add faenza-icon-theme



# Desarrollo
-------------------------------------------

Es importante destacar que para los que tienen intenet lento o deficiente, 
siempre instalar no solo `<paquete>` sino tambien `<paquete>-doc` para tener 
al menos una pista de documentacion ya que la wiki de alpine es una basura.

## Desarrollo base

```
apk add autoconf bison flex flex-libs graphviz-dev nettle-dev
apk add build-base abuild binutils binutils-doc
apk add make make-doc cmake cmake-doc txt2man
```

## Repositorios git cvs svn mvn

```
apk add git git-doc git-bash-completion
apk add subversion subversion-doc
apk add mercurial mercurial-doc
```

## Bases de datos

```
apk add sqlite sqlite-libs sqlite-doc
apk add mariadb mariadb-server-utils 
```

## Desarrollo C y C++

```
apk add gcc gcc-objc gcc-gnat gcc-doc g++
apk add glade-dev glade3-dev glib-dev glib-bash-completion glibmm-dev gtk+3.0-dev gtk+2.0-dev gtkmm-dev gtkmm3-dev
```

## Desarrollo golang

```
apk add go go-gdm go-tools go-bootstrap go-doc dep
```

**IMPORTANTE** los paquetes `glide` y `godep` ya no son usados **no emplearlos sino el `deb`**

## Desarrollo web

```
apk add php5
apk add lighttpd
apk add python2-dev python3-dev
```


# Soporte multidistro
-------------------------------------------

## Desarrollo arch/geento

```
apk add paxctl build-base binutils
```

## Desarrollo debian

```
apk add debootstrap
export debflav=lenny
mkdir -p /srv/debian/chroot-$debflav
debootstrap --arch=i386 $debflav /srv/debian/chroot-$debflav http://archive.debian.net/debian/
chroot /srv/debian/chroot-$debflav /bin/bash
```

**NOTA** para alpine 2.4 o 2.6 necesitaras: `for i in /proc/sys/kernel/grsecurity/chroot_*; do echo 0 | tee $i; done`

