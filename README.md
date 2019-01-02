https://mckayemu.github.io/alpineinstalls/

# alpineinstalls

Documentos en español por periodos, para la experiencia usuario con alpine. 
Estos documentos se enfocan mas en multimedia, asi como de solo dos escritorios LXDE/openbox y lxqt/xfce, 
ya que mate esta muy casado con systemd.

### Inicio:

Alpine permite instalar desde disco, usb, o red, en las dos primeras usa una imagen arrancable mientras 
que en la tercera se necesita una imagen dumpeada o un Alpine desde otra maquina. Por facilidad, 
asumiremos es la primera vez y asumiremos las dos primeras.

[instalar/instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md](instalar/instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md)
Este documento asume tiene un discoduro ya particionado, el cdrom ya grabado (o usb), para adicionar Alpine, 
pero el arranque no sera desde Alpine, sino desde otro linux mas facil y estable de usar, 
este documento tambien particiona customizado usando optimizacion y MBR del disco en vez de UEFI.

Para mejores u/o otras combinaiones de instalacion leer [instalar/README.md](instalar/README.md)

## Instalar programas

En alpine los programas se instalan desde la red, pienselo, aunque compre un CD este no esta en su sistema, 
igual es con los paquetes, estos no estan en su sistema, si no tiene internet se puede descargar todos 
en un directorio y instalar desde alli con una simple linea.

**Importante** al igual que ocurre en Debian y otras, muchos paquetes estan "cortos" como ejemplo, si 
comparamos `ffmpeg` de Debian vs el de Marillat el de Debian sabemos es basura, pasa peor en Alpine, 
los paquetes no siempre son compilados por expertos y pueden esten sin uno que otro soporte, esto 
es muy muy frecuente en los paquetes multimedia y de juegos.

WIP repo local y disco a distribuir con todo para no depender de internet (y con paquetes bien hecho no los chuutos incompletos)

### QUE VERSION ME CONVIENE PARA QUE HARDWARE

Dado Alpine usa kernel 4.X y compila dependeindo la disponibilidad de los colaboradores 
este requiere de hardware minimo desde 2010 dejando mucho por detras, si ud posee 
hardware por ejemplo Semprom, K6-III, Pentium4, PentiumIII, PentiumII nopuede instalar 
una version mas alla de Alpine 3.2 (este es el ultimo con kernel 3.X).

Para mayor informacion consultar la tabla de hardware y alpines recomendados:
* [informes/hardware-y-versiones-alpine-recomendados.md](informes/hardware-y-versiones-alpine-recomendados.md)

> No pero yo tengo Alpine isntalado en mi Pemtium 4 y funciona bien

Falso: desde kernel 4 el soporte de speedstep quedo deprecated para estos procesadores. 
la solucion es instalar Alpine 3.2 que tiene kernel 3.X y aun soporte bien maquinas viejas.

# Aclaratorias, ventajas y deficiencias

Es importante que para algunos que se creen expertos linux Alpine es para machos, 
sin embargo difiere y tiene algunas desventajas asi como cambios respectos otras distros.

Las controversias sobre estabilidad y garantias viene dado por las dos mas grandes diferencias, 
el uso de musc libc y el que es una rolling release y una nueva version de programa puede romper otro 
sino se maneja el que tambine se tendra que actualizar sus dependencias, pro lo general 
el que empaqueta es solo a manera de colaboracion y cuando no es experto sucede esto, 
ya que no es una distro extremadamente usada no se cubren todos los casos de uso (importante acotacion).

1. **La libreria central de C es musc libc** es la principal diferencia, si bien permite 
mucha mayor rapidez tambine es un dolor de cabeza, ya que imposibilita tener otros idiomas 
configurados de manera dual ademas de imposibilitar compilar mcuhos programas, muchos 
no asuma que todo compilara comoud cree conocer, esto no es Winbuntu.. debera leer.
2. **El boot manager es syslinux** no asuma es igual a otras distros, syslinux es mas 
rapido que Grub pero mas delicado, puesto en cada modificacion debe volver escribirse el MBR. 
Cabe destacar que para dual boot es recomendable usar el directorio `/etc/update-extlinux.d/` 
mediante la colocacion de archivos ".conf" se puede usar numeros para ello, la sintaxis debe 
ser igual que la de syslinux para entradas, en nuestros documentos no apoyaremos syslinux 
por ser demasiado delicado y exigir escribir en el MBR en cada cambio.
3. **Para EFI debe cambiar el bootmanager**, no se recomienda usar EFI/UEFI y desactivelo, 
lamentablemente como toda distro minimalista su spoporte UEFI es cortado y deficiente, 
desactive el UEFI y mejor use particionamiente MBR en vez de GPT para evitar problemas. 
Hoy dia el instalador no escribe correctamente todos los UEFI boot, asi que no lo use.
4. **El modo automatizado de instalacion es ineficiente** igual como si fuese Winbuntu, 
esto porque en ningun momento permite interrumpirlo para customizar, por ejemplo en el 
caso de Debian, se permite la instalacion automatica interrumpirla para realizar ajustes, 
en el caso Alpine no permite customizar el disco duro ni el esquema de particiones 
a menos se haga todo el proceso de instalacion y bootmanager de manera super manual.
5. **Versiones rolling releases** asi que una nueva version de cualqueir programa, 
debe tener cuidado al actualizar, porque puede que su hardware o su configuracion deje 
de funcionar repentinamente, al no ser como Debian esto es lo normal en este tipo de 
distribuciones.. con la unica ventaja que esta siempre al dia pero no garantiza unicidad.


