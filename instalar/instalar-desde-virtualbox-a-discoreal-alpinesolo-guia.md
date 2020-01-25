Debido a que no tenemos usb ni imagen alpine, ni cdrom usaremos virtualbox, 
la instalacion sera automatizada, creara unmonton de particiones innecesarias 
para customizaciones vease al final la seccion de "Vease tambien".


## preparar medios para instalar

Para esto creamos una unidad que mapee el disco real como virtual y despues crear 
una maquina virtualbox que lo utilize, todo lo haremos desde virtualbox:

```
VBoxManage internalcommands createrawvmdk -rawdisk /dev/sdb -filename /home/systemas/VirtualBox\ VMs/rawdisk-sdb.vmdk
```

**NOTA:** lamentablemente el instalador de alpine automatiza la creacion de particiones, 
esto es ineficiente porque no permite customizar el tamano de bloques, el cual se creara 
con uno de 4096, pero como el sistema usa archivos pequeño el optimo es 2048 y no ese.

Despues configuramos una maquina virtual sin sonido ojo, y arrancamos con ese disco, 
Al iniciar Alpine preguntara por el login, solo escribir `root` y pulsar enter permite iniciar:

![instalar-desde-virtualbox-a-discoreal-dualboot-screenshot-01.png](instalar-desde-virtualbox-a-discoreal-dualboot-screenshot-01.png)


## instalador

Alpine usa una version cortada de Libc llamda `musl` desde la 3.X, en 
las versiones viejas usaba `uClibc` ambas siempre ponen el teclado a inglesh, 
asi que cuidado con lo que escribe esto no es winbuntu, esto es linux okey:

**ADVERTENCIAS** la cagada como todas las minimalistas es que se requiere internet, 
a menos tengas una imagen ya instalada y simplemente la clones en el disco. 
asi que **necesitas internet si es primera vez o no tienes imagen alpine**.

```
export BOOT_SIZE=500
export SWAP_SIZE=4096
export BOOTLOADER=grub

setup-alpine
```

No usar ningun flag, solo el comando, despues de contestar "sys" a las preguntas sobre el 
disco, y como solo existira un solo disco contestar "sda" en que disco usar.

Esto creara y dejara su disco duro de la siguiente manera:

* `/dev/sda1` como BOOT en 500Mb en `/boot`
* `/dev/sda2` como SWAP en 4Gb
* `/dev/sda3` como ROOT en 200Gb en `/` (aproximadamente)

En pocos minutos estara ya todo listo para usar, se recomeinda leer aclaratorias:

# Aclaratorias y deficiencias

1. **El boot manager es syslinux** aqui se customizo a grub, dado que, syslinux es mas 
rapido que Grub pero no soporta bien UEFI, puesto en cada modificacion debe volver escribirse el MBR. 
Cabe destacar que para dual boot es recomendable usar el directorio `/etc/update-extlinux.d/` 
mediante la colocacion de archivos ".conf" se puede usar numeros para ello, la sintaxis debe 
ser igual que la de syslinux para entradas, en nuestros documentos no apoyaremos syslinux 
por ser demasiado delicado y exigir escribir en el MBR en cada cambio.
2. **Para EFI en versiones viejas anteriores a la 3.9 se debe cambiar el bootmanager**, si puede 
no se recomienda usar EFI/UEFI y desactivelo, desde Alpine 3.9.2 ya esto es automatico, 
lamentablemente como toda distro minimalista su spoporte UEFI es cortado y deficiente, 
desactive el UEFI y mejor use particionamiente MBR en vez de GPT para evitar problemas. 
Hoy dia el instalador utiliza grub si necesita emplear soporte UEFI pero deben existir las particiones.
3. **El modo automatizado de instalacion es ineficiente** igual como si fuese Winbuntu, 
esto porque en ningun momento permite interrumpirlo apra customizar, en el 
caso de Debian, se permite el instalcion automatica interrumpirla para realizar ajustes, 
en el caso Alpine no permite customizar el disco duro ni el esquema de particiones.

Para cambiar alguno de estos lease la siguiente seccion:

# Vease tambien:

* [README informacion general](../README.md)
* [instalar-desde-usb-cdrom-alpinesolo-guia.md](instalar-desde-usb-cdrom-a-discoreal-alpinesolo-guia.md)
* [instalar-desde-virtualbox-a-discoreal-dualboot-guia.md](instalar-desde-virtualbox-a-discoreal-dualboot-guia.md)
* [instalar-desde-virtualbox-a-discoreal-dualboot-guia-grub.md](instalar-desde-virtualbox-a-discoreal-dualboot-guia-grub.md)
