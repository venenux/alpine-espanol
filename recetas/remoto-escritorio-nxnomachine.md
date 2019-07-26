# nxnomachine en alpine

Este es un trabajo en progreso.. aun no se ha logrado terminar.. 
se agradece colaboracion en el asunto.. 

### Requisitos

Tener instalado como minimo:

* linuxutils: `apk add wget attr coreutils grep util-linux pciutils usbutils binutils lsof less curl`
* xorg: `apk add udev gtkglext pango atk cairo gdk-pixbuf mesa-gl libxcb libxrandr libxv libxxf86vm libxxf86misc libxmu`
* desktop: `apk add lxdm openbox` (instalar aqui el que guste pero debe haber uno que defina `DESKTOP_SESSION`)
* awk: `apk add gawk`
* usb: `modprobe vhci-hcd; apk add linux-headers linux-vanilla-dev build-base linux-virt-dev`
* pam: `apk add linux-pam`
* gcc: `apk add gcc gcc-objc gcc-gnat gcc-doc g++`
* automake: `apk add make m4 automake autoconf bison flex flex-libs`
* crypo: `openssl openssl graphviz-dev nettle-dev`

### Descargar nomachine

```
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


## NOTAS:

nxmachine compila un kerne para comparticion remota `vhci_hcd` que ya esta.. 
se puede hackear colocando `release 6.0` en `/etc/issue` .. evitando problemas.
