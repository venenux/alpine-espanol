
En este directorio se encontraran varias guias de inicio e instalacion para Alpine, 
esto porque es muy carente la informacion en español de Alpine ademas de incorrecta.

**SOPORTE UEFI** este esta soportado experimentalmente desde alpine 3.6 y 
es completamente soportado en alpine 3.10 instala grub por defecto pero 
requiere minimo 4 particiones para que funcione.

Las controversias sobre usabilidad se pueden leer en el index README, esto 
viene dado por las dos mas grandes diferencias, el uso de musc libc y la 
carencia de empaquetadores siendo una distro algo pequeña delegada a dockers.

![instalar-desde-virtualbox-a-discoreal-dualboot-screenshot-01.png](instalar-desde-virtualbox-a-discoreal-dualboot-screenshot-01.png)

# Guias que usan una imagen de instalacion en computadores

1. [instalar-desde-usb-a-discoreal-alpinesolo-computadora.md](instalar-desde-usb-a-discoreal-alpinesolo-computadora.md) 
Este documento le enseña como grabar el medio de instalacion en un USB drive 
para instalarlo en su computadora usando todo el disco duro solamente para Alpine, 
dejando el computador unicamente con Alpine Linux como sistema operativo.
2. [instalar-desde-cdrom-a-discoreal-alpinesolo-computadora.md](instalar-desde-cdrom-a-discoreal-alpinesolo-computadora.md).
Este documento le enseña como grabar el medio de instalacion en un CD/DVD en blanco 
para instalarlo en su computadora usando todo el disco duro solamente para Alpine, 
dejando el computador unicamente con Alpine Linux como sistema operativo.
3. [instalar-desde-imagen-a-virtualbox-alpinesolo-computadora.md](instalar-desde-imagen-a-virtualbox-alpinesolo-computadora.md)
Este documento le enseña sin usar USB ni CD/DVD usando VirtualBox unicamente, 
para instalarlo usando un disco virtual dentro de su computadora para Alpine, 
realizando el proceso desde una maquina virtual.
4. [instalar-desde-virtualbox-a-discoreal-alpinesolo.md](instalar-desde-virtualbox-a-discoreal-alpinesolo.md)
Este documento asume tiene un discoduro para borrar y solo colocarle Alpine Linux, 
pero no tiene un cd ni usb donde colocar el medio desde donde va instalar,
realizando el proceso desde una maquina virtual hacia el disco real, 
requerira una baiha externa o un puerto sata esclavo dentro de la pc.
5. [instalar-desde-virtualbox-a-discoreal-dualboot-guia.md](instalar-desde-virtualbox-a-discoreal-dualboot-guia.md)
Este documento asume tiene un discoduro pero solo una parte para Alpine Linux, 
pero no tiene un cd ni usb donde colocar el medio desde donde va instalar,
realizando el proceso desde una maquina virtual hacia el disco real, 
requerira una baiha externa o un puerto sata esclavo dentro de la pc.
6. [instalar-desde-usb-cdrom-alpinesolo-guia-optimizado.md](instalar-desde-usb-cdrom-alpinesolo-guia-optimizado.md) (WIP)
Este documento asume tiene un discoduro, cdrom ya grabado (o usb) y lo borrara todo, para solo tener alpine, 
pero optimiza la particion customizado usando optimizacion y MBR del disco en vez de UEFI.

# Guias que instalan sin media de instalacion pero desde otro linux

7. [instalar-desde-debian-internet-alpine-chroot-directorio-directorio.md](instalar-desde-debian-internet-alpine-chroot-directorio-directorio.md) 
Este documento permite usar Alpine desde dentro de otro linux sin reinicar, 
en este caso Debian o Devuan es el sistema principal y no hay que reinicar.
8. [instalar-desde-alpine-internet-alpine-chroot-directorio-directorio.md](instalar-desde-alpine-internet-alpine-chroot-directorio-directorio.md) 
Este documento permite usar Alpine desde dentro de otro linux sin reinicar, 
en este caso Debian o Devuan es el sistema principal y no hay que reinicar.

# Guias que instalan por clonado o sin usar internet (WIP)

9. [instalar-desde-imagen-clone-alpine-avanzado.md](instalar-desde-imagen-clone-alpine-avanzado.md) 
Este es un documento de nivel medio que permite usar un imagen de alpine linux 
y duplicarla en otro computador usando copia de discos duros y clonezilla.
10. [instalar-desde-imagen-redes-alpine-avanzado.md](instalar-desde-imagen-redes-alpine-avanzado.md) 
Este es un documento de nivel avanzado que permite usar un imagen de alpine linux 
y duplicarla en otro computador usando la red interna.

# Guias de imagenes en dispositivos embebidos (WIP)

11. WIP rasberry pi

# Vease tambien:

* [README informacion general](../README.md)
* [Receta despues de instalar: configuracion y paquetes](../recetas/alpine-recetas-configuracion-y-paquetes-sistema.md)
* [programas-esenciales-todo-en-uno.md](../recetas/programas-esenciales-todo-en-uno.md)
* [Informes y tablas de compatibilidad](../informes/hardware-y-versiones-alpine-recomendados.md)

