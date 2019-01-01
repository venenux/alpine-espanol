
Debido a que no tenemos usb ni imagen alpine, ni cdrom usaremos virtualbox, 
ojo esto necesita que el grup y vmimage sean con todo incluido.
Para esto creamos una unidad que mapee el disco real:

```
VBoxManage internalcommands createrawvmdk -rawdisk /dev/sdb -filename /home/systemas/VirtualBox\ VMs/rawdisk-sdb.vmdk
```

Despues configuramos una maquina virtual sin sonido ojo, y arrancamos con ese disco:

![instalar-desde-virtualbox-a-discoreal-01.png](instalar-desde-virtualbox-a-discoreal-01.png)

## Configurar disco en donde instalar

A diferencia de otros sistemas de instalacion, el de alpine es automatico en el disco, 
y monta un viaje de particiones innecesarias, para hacer una configuracion manual 
hay que ejecutar `fdisk`, particionar, y despues montarlo para `setup-disk` y alli indicar.

#### 1. Particionar

Se advierte dejar la estupidez de particionamiento complicado, no hay ninguna necesidad 
de separar boot y demas, solamente home, porque se supone el linux es perfecto y no se jode!
En debian desde que uso squeeze no separo para mis desktop solo en servidores.
La separacion era solo porque a medida se instalaba o necesitaba espacio no se 
podia apagar la maquina y de paso los discos eran lentos y pequeños... 
por ello existia las particiones y el LVM, cosa que hoy es innecesria en desktop..

**IMPORTANTE** colocar LVM hace mas lento, se aprecia cuando trabajas con java y eclipse 
y las maquinas sin LVM son EVIDENTEMENTE mas rapidas. Otra forma de evidenciarlo 
es usando un disco SSD, porque son mas rapidos, obvio el acceso es mas rapido.. 
pienselo, LVM es una capa mas sobre las ya capas de manejo de sistema de ficheros...

```
fdisk /dev/sda
Command (m for help): p
Disk /dev/sdb: 298,1 GiB, 320072933376 bytes, 625142448 sectores
Identifier: 0x0009b7e5

Device     Boot    Start       End   Sectors   Size Id Type
/dev/sdb1  *          63  41945714  41945652    20G 83 Linux
/dev/sdb2       41945715  83907494  41961780    20G 83 Linux
/dev/sdb3       83907495  88116524   4209030     2G 82 Linux swap / Solaris
/dev/sdb4       88116525 625105214 536988690 256,1G 83 Linux


Command (m for help): q
```

El disco se le borrara toda particion, si queire usar dualboot (WIP guia dual boot)
secuencia de comandos OJO ESTAMOS USANDO MBR, para GPT es distinto, usaremos MBR, porque 
ahorra unos microsegundos, esto porque la GPT tiene dos capas, la que ve el OS y 
una extra que emula la MBR para que los OS viejos vean una sola particion que 
indique que es del tipo GPT, esta capa de compatibilidad es tiempo muerto, asi 
que usaremos MBR con solo particiones primarias, las extendidas tienen el mismo 
defecto ya que necesitan un indice extra para indicar cuantas extendidas hay:

* Pulsar `d` al pedir numero pulsar `4` y borrara la particion 4.
* Pulsar `d` al pedir numero pulsar `3` y borrara la particion 3.
* Pulsar `d` al pedir numero pulsar `2` y borrara la particion 2.
* Pulsar `d` al pedir numero pulsar `1` y borrara la particion 1.
* Pulsar `n` y al preguntar entre "e" y "p" pulsar `p` siempre usar primaria
* Preguntara numero particion pulsar `1` que sera la root
* Preguntara "First cilinder" usar `2` no usar 1. En Last cilinder usar el tamaño asi: `+20G` para gigas
* Pulsar `n` y al preguntar entre "e" y "p" pulsar `p` siempre usar primaria
* Preguntara numero particion pulsar `2` que sera la root
* Preguntara "First cilinder" usar `Enter` sin ingresar nada. En Last cilinder usar el tamaño asi: `+2G` para swap
* Pulsar `n` y al preguntar entre "e" y "p" pulsar `p` siempre usar primaria
* Preguntara numero particion pulsar `3` que sera la home
* Preguntara "First cilinder" usar `Enter` sin ingresar nada. En Last cilinder usar el ultimo numero menos uno!

**NOTA1: swap** no usar una swap de mas de 2G es ilogico.. leer documentacion redhat y diseño del kernel.
**NOTA2: home** no usar nunca el primer o ultimo cilindro en discos, esto causa perdida de datos en desmonturas forzadas (ida de luz)
**NOTA3** en las laptops es mas que innecesario una swap grande o LVM ya que las pone mas lentas.

El mapa de disco quedara asi par mi caso de 320G :

```
Device     Boot    Start       End   Sectors   Size Id Type
/dev/sdb1  *          63  41945714  41945652    20G 83 Linux
/dev/sdb2       41947136  46141439   4194304     2G 83 Linux
/dev/sdb3       46141440 625142447 579001008 276,1G 83 Linux
```

#### 2. Configurar disco

Una vez particionado debemos preconfigurar el `setup-disk` con el 
destino que se usara. Para esto formateamos, montamos y ejecutamos indicacion:

```
apk add e2fsprogs
mkfs.ext3 /dev/sda1 -L alp1root -b 2048
mount -t ext3 /dev/sda1 /mnt
```

Esto dejara el disco alli pendiente, el instalador le indicaremos no usarlo para despues, reconfigurar que se usara.

**IMPORTANTE** ya que usaremos archivos pequenos mas que grandes, la raiz usara "2048" 
en vez de "4096" permitiendo mas fragmentacion y asi encontrando mas rapido la escritura. 
En el caso de la particion home esta si dejarla con 4096 ya que alli ud guardara su ~porn~ digo peliculas, 
Esto es porque alocar un bloque grande para un archivo pequeño hace que las busquedas sean lentas, 
el sistema operativo tiene muchos archivos pequeños asi que por eso usar un tamaño de bloque pequeño.

![instalar-desde-virtualbox-a-discoreal-02.png](instalar-desde-virtualbox-a-discoreal-02.png)

## instalador

Alpine usa una version cortada de Libc llamda `musl` desde la 3.X, en 
las versiones viejas usaba `uClibc` ambas siempre ponen el teclado a inglesh, 
asi que cuidado con lo que escribe esto no es winbuntu, esto es linux okey:

**ADVERTENCIAS** la cagada como todas las minimalistas es que se requiere internet, 
a menos tengas una imagen ya instalada y simplemente la clones en el disco. 
asi que **necesitas insternet si es primera vez o no tienes imagen alpine**.

**IMPORTANTE** debes cuando llegue a la parte del disco "Which disks yous you like to use?" 
contestar "none",de alli en adelante contestar "none" hasta salir, esto para permitir 
la configuracion customizada de disco.


```
setup-alpine
```

No usar ningun flag, solo el comando, despues de contestar "none" a las preguntas sobre el 
disco, ejecutar el configurador del disco para colocar lso archivos en el disco:

```
setup-disk -m sys /mnt
```

Esto copiara el sistema al disco pero recordemos tenemos el sistema en uan sola particion 
y alpine es marico con eso, asi que hay que ajustarlo en el bootmanager config asi:

```
apk add nano
nano /mnt/boot/extlinux.conf
```

A los parametros `KERNEL` y `APPEND` hay que indicar es desde el directorio `/boot/` 
del disco iniciado adicionando en "KERNEL y agregando en "APPEND" en el parametro "initrd".
En la imagen se muestra este archivo, usar como referencia.

![instalar-desde-virtualbox-a-discoreal-01.png](instalar-desde-virtualbox-a-discoreal-08.png)

![instalar-desde-virtualbox-a-discoreal-01.png](instalar-desde-virtualbox-a-discoreal-10.png)

![instalar-desde-virtualbox-a-discoreal-01.png](instalar-desde-virtualbox-a-discoreal-11.png)

