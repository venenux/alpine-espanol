
Esto es escritorio, no servidor, en escritorio se necesita de todo, la seguridad es solo 
en dos lugares, el naveador y el ssh (ya que no montaremos ningun sistema web como servicio mas que desarrollar).

Este documento asume minimo Alpine 3.3 y maximo alpine 3.9 para otras versiones puede o no funcionar.

* Si algo no funciona corrobore: [hardware-y-versiones-alpine-recomendados.md](hardware-y-versiones-alpine-recomendados.md)


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

apk add attr dialog dialog-doc bash bash-doc bash-completion coreutils coreutils-doc grep grep-doc
apk add util-linux util-linux-doc pciutils usbutils binutils findutils readline
apk add man man-pages lsof lsof-doc less less-doc nano nano-doc curl curl-doc
export PAGER=less

apk add python2 python3

apk add htop testdisk btrfs-progs-extra btrfs-progs-doc e2fsprogs-extra e2fsprogs-doc dosfstools dosfstools-doc
apk add imagemagick xorriso rsync antiword asciidoc wv wv-doc
apk add aspell aspell-en aspell-ru aspell-de aspell-utils aspell-doc aspell-libs

apk add cpufrequtils
rc-service cpufrequtils start
rc-update add cpufrequtils
```

## decompresion y compresion

Para interfaz grafica de compresion y decompresion ir a la seccion de escritorio.

```
apk add tar zlib xz zip unzip p7zip cpio lha sharutils lzip lzo gzip rpm cabextract
apk add tar-doc zlib-doc xz-doc zip-doc p7zip-doc cpio-doc lha-doc sharutils-doc gzip-doc rpm-doc
```

## comunicacion e internet consola

```
apk add wpa_supplicant
rc-update add wpa_supplicant default
apk add irssi irssi-doc irssi-proxy fish weechat weechat-aspell weechat-lua weechat-python weechat-perl
apk add aria2 wget wget-doc elinks elinks-doc macchanger macchanger-doc 
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
rc-update add udev
rc-service udev start
apk add xorg-server xorg-server-xephyr xf86-video-vesa xf86-video-intel xf86-input-evdev  xinit
apk add xf86-input-mouse xf86-input-synaptics xf86-input-keyboard font-util font-util-doc fontconfig
```

## Placas de video

1). Debemos agregar soporte 3d asi sea falso, hoy dia la estupida dependencia lo exige:

```
apk add glib libxcb libxrandr libxv libxxf86vm libxxf86misc libxmu xcb-util-cursor xcb-util-image xcb-util-keysyms
apk add libva libva-glx mesa mesa-gl mesa-egl mesa-gles mesa-gbm mesa-glapi mesa-osmesa xf86-video-modesetting
```

**NOTA** no instalar kbd si tiene laptop a menos que sepa lo que hace

2). Despues segun al tarjeta/placa de video OJO mo mexclar los "mesa-dri-xxx":

* Intel:

```
apk add xf86-video-intel mesa-dri-intel mesa-vulkan-intel libva-intel-driver
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
apk add gtkglext pango atk cairo gdk-pixbuf gtk+2.0 gtk+3.0 wxgtk gtk-engines gtk-engines-industrial 
apk add gtkmm atkmm pangomm cairomm gtkmm3 hicolor-icon-theme imagemagick-c++ imagemagick-libs ghostscript
apk add shared-mime-info gtk-murrine-engine gtk-xfce-engine xrandr
apk add ttf-dejavu ttf-droid ttf-freefont ttf-liberation ttf-linux-libertine ttf-ubuntu-font-family
apk add lxdm scrot
```

## Multimedia sonido y video

```
apk add alsa-utils alsa-utils-doc alsa-lib alsaconf
rc-service alsa start
rc-update add alsa
apk add bluez bluez-firmware bluez-libs bluez-hid2hci bluez-doc bluez-cups bluez-deprecated
rc-service bluetooth start
rc-update add bluetooth
apk add gstreamer0.10 gstreamer0.10-tools gstreamer0.10-docs gst-plugins-base0.10 gst-plugins-base0.10-doc
apk add gstreamer gstreamer-tools gstreamer-doc gst-plugins-base gst-plugins-base-doc
apk add dvd+rw-tools cdparanoia cdrkit cdw 
apk add portaudio lame lame-doc vorbis-tools openal-soft optipng ffmpegthumbnailer wxgtk-media exiv2-doc exiftool
apk add sdl sdl-doc sdl_image sdl_mixer sdl2 sdl2_image sdl2_mixer sdl2_ttf

```

## Escritorio LXDE y openbox

```
apk add dbus dbus-x11 dbus-glib dhcpcd-dbus dbus-libs dbus-doc dbus-glib-doc
rc-service dbus start
rc-update add dbus
apk add arandr fuse fuse-doc sshfs gvfs gvfs-afp gvfs-archive gvfs-dav gvfs-fuse gvfs-mtp
apk add desktop-file-utils gnome-vfs gnome-keyring gnome-power-manager zenity compton compton-doc
apk add openbox wbar gpicview leafpad gucharmap pcmanfm ghostscript fbpanel terminator terminator-doc
apk add gnumeric abiword abiword-doc abiword-plugin-gimp abiword-plugin-freetranslation abiword-plugin-presentation abiword-plugin-pdf abiword-plugin-google
apk add aumix deadbeef x265 x264 ffmpeg gnomad2 gnome-bluetooth mpv mpv-doc mpv-libs
apk add guvcview inkscape-view inkscape-doc youtube-dl espeak gimp gimp-doc gphoto2
apk add font-adobe-75dpi font-adobe-100dpi font-adobe-utopia-75dpi
apk add font-bitstream-75dpi font-bitstream-100dpi font-bitstream-type1
apk add font-xfree86-type1 font-cronyx-cyrillic font-misc-cyrillic font-croscore font-noto
apk add gst-plugins-ugly gst-plugins-ugly0.10 gst-plugins-bad gst-plugins-bad0.10

apk del lxpolkit
apk add lxdm lxsession openbox-doc xdg-utils 
apk add faenza-icon-theme
```

## Escritorio internet y redes

```
apk add wpa_supplicant
rc-update add wpa_supplicant default
rc-service wpa_supplicant start
apk add modemmanager usb-modeswitch usb-modeswitch-udev 
apk add networkmanager network-manager-applet network-manager-applet-doc networkmanager-doc
rc-update add networkmanager default
rc-service networkmanager start
apk add pidgin pidgin-doc 
apk add gigolo firefox-esr
```


## Documentos y oficina

```
apk add cups bluez-cups cups-filters cups-doc
rc-service cupsd start
rc-update add cupsd
apk add libreoffice libreoffice-impress libreoffice-calc libreoffice-writer libreoffice-math libreoffice-gnome
```


# Desarrollo
-------------------------------------------

Es importante destacar que para los que tienen intenet lento o deficiente, 
siempre instalar no solo `<paquete>` sino tambien `<paquete>-doc` para tener 
al menos una pista de documentacion ya que la wiki de alpine es una basura.

## Desarrollo base

```
apk add m4 automake autoconf autoconf2.13 automoc4 bison flex flex-libs graphviz-dev nettle-dev
apk add build-base abuild binutils binutils-doc nasm
apk add make make-doc cmake cmake-doc txt2man
apk add sphinx-php sphinx-python sphinx-doc docbook-xsl docbook-xml docbook2x help2man
apk add openvpn
```

## Repositorios git cvs svn mvn

```
apk diffutils diffutils-doc
apk add git git-doc git-bash-completion
apk add subversion subversion-doc
apk add mercurial mercurial-doc
apk add bzr bzr-doc maven cvs
```

## Bases de datos

```
apk add unixodbc db db-c++ psqlodbc
apk add sqlite sqlite-libs sqlite-doc
apk add mariadb mariadb-server-utils 
```

## Desarrollo C y C++

```
apk add gcc gcc-objc gcc-gnat gcc-doc g++
apk add glade-dev glade3-dev glib-dev glib-bash-completion glibmm-dev gtk+3.0-dev gtk+2.0-dev gtkmm-dev gtkmm3-dev wxgtk-dev
```

## Desarrollo golang

```
apk add go go-gdm go-tools go-bootstrap go-doc dep
```

**IMPORTANTE** los paquetes `glide` y `godep` ya no son usados **no emplearlos sino el `deb`**

## Desarrollo web

```
apk add lighttpd lighttpd-doc lighttpd-mod_webdav lighttpd-mod_auth fcgi fcgi++ 
apk add nodejs nodejs-npm

apk add pho5-cgi php5-sockets php5-openssl php5-iconv php5-mcrypt php5-json php5-iconv
apk add php5-cli php5-curl php5-bcmath php5-bz2 php5-calendar php5-ctype php5-doc php5-imap
apk add php5 php5-odbc php5-mysql php5-pgsql php5-sqlite3 php5-pdo php5-dom 
apk add php5-pear php5-phar php5-zlib php5-zip php5-xmlreader php5-cmlrpc php5-xls php5-pspell

apk add phpmyadmin phpmyadmin-doc webkitgtk webkit2gtk

apk add perl-cgi-fast perl-cgi-doc perl-cgi-session perl-dbd-odbc perl-dbd-sqlite perl-dbd-mysql perl-dbd-pg

apk add python2-dev python3-dev py-django py-django-compressor py-django-oscar py-websocket-client
```

## Desarrollo lua

WIP demasiado grande hay que ver los paquetes que metan todo

## IDES y entornos

```
apk add dia dia-doc
apk add libreoffice-base
apk add geany geany-plugins-addons geany-plugins-geanyextrasel geany-plugins-geanylua geany-plugins-geanyprj
apk add geany-plugins-geanypy geany-plugins-geanyvc geany-plugins-treebrowser
```

## Desarrollo Java y JEE

```
apk add openjdk7 openjdk8 fastjar
```


# Soporte multidistro
-------------------------------------------

## Desarrollo arch/geento

```
apk add paxctl build-base binutils
```

## Desarrollo debian

```
apk add debootstrap dpkg dpkg-dev dpkg-doc
export debflav=lenny
mkdir -p /srv/debian/chroot-$debflav
debootstrap --arch=i386 $debflav /srv/debian/chroot-$debflav http://archive.debian.net/debian/
chroot /srv/debian/chroot-$debflav /bin/bash
```

**NOTA** para alpine 2.4 o 2.6 necesitaras: `for i in /proc/sys/kernel/grsecurity/chroot_*; do echo 0 | tee $i; done`


# Vease tambien:

* [../README-md (indice)](../README-md)
* [Informaciones y notas](../informes/)
