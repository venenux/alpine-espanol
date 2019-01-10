
# nxnomachine en alpine


### Requisitos

Tener instalado como minimo:

* xorg: 
`apk add gtkglext pango atk cairo gdk-pixbuf mesa-gl libxcb libxrandr libxv libxxf86vm libxxf86misc libxmu`
* desktop: 
`apk add lxdm openbox` (instalar aqui el que guste pero debe haber uno que defina `DESKTOP_SESSION`)
* usb
* gcc
* 

### Descargar nomachine

```
apk add wget attr coreutils grep util-linux pciutils usbutils binutils lsof less curl

wget https://download.nomachine.com/download/6.4/Linux/nomachine_6.4.6_1_i686.tar.gz
tar -C /usr/ -xzf nomachine_6.4.6_1_i686.tar.gz
```

**IMPORTANTE** cada vez umenta la version el tarball desaparece por una nueva.. 
en este ejemplo la ultima era 6.4.6 pero si sale uan nueva el enlace cambiara 
y ya no funcionara.


### Instalar el modulo servidor


```
/usr/NX/nxserver --install redhat
```