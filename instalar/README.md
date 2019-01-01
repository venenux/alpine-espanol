
En este directorio se encontraran varias guias de inicio e instalacion para Alpine, 
esto porque es muy carente la informacion en español de Alpine ademas de incorrecta:

1. [instalar-desde-usb-cdrom-alpinesolo-guia.md](instalar-desde-usb-cdrom-alpinesolo-guia.md)
Este documento asume tiene un discoduro, cdrom ya grabado (o usb) y lo borrara todo, para solo tener alpine, 
el particionamiento sera automatizado, lo que nolo optimizara y colocara mas de una particion para escritorio.
2. [instalar-desde-virtualbox-a-discoreal-dualboot-guia.md](instalar-desde-virtualbox-a-discoreal-dualboot-guia.md)
Este documento asume tiene un discoduro pero sin usb o cdrom y lo borrara todo, para solo tener alpine, 
pero el arranque no sera desde Alpine, sino desde otro linux mas facil y estable de usar, 
este documento tambien particiona customizado usando optimizacion y MBR del disco en vez de UEFI.
3. [instalar-desde-virtualbox-a-discoreal-dualboot-guia-grub.md](instalar-desde-virtualbox-a-discoreal-dualboot-guia-grub.md)
Este documento asume tiene un discoduro pero sin usb o cdrom y lo borrara todo, para solo tener alpine, 
pero se modificara para usar Grub desde el mismo Alpine y sera Alpine quien comande el arranque, 
este documento tambien particiona customizado usando optimizacion y MBR del disco en vez de UEFI.

Las controversias sobre la estabilidad se pueden leer en el index README, esto 
viene dado por las dos mas grandes diferencias, el uso de musc libc y el que 
es uan rolling release y un nuevo version de programa puede romper otro sino se maneja.

# Vease tambien:

* [README informacion general](../README.md)
* [Entorno grafico](../programas/README-escritorios.md)
* [Instalacion de programas](../programas/README.md)
