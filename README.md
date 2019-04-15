https://mckayemu.github.io/alpineinstalls/

# alpineinstalls

**Sitio en español para la experiencia usuario con alpine** como consecuencia de su asquerosa wiki.
Esto se enfoca solo en escritorio, multimedia, ya que para servidores un verdadero linuxero sabra que hacer..
La razon es obvia, si quiero un servidor monto "yerro" en un AWS, ahora para maquinas viejas 
si no arranca un escritorio es ilogico porque como haces que otra persona te ayude?.
**Por eso este sitio, en español, hacer arrancar un Alpine en esas maquinas y tener un escritorio decente, 
y no esas carcachas que hay que hacer malabares para poder usar la wifi, algo que con dos click hasta una cucaracha hace hoy dia!**

* [Inicio aqui: instalar alpine con varias recetas](#inicio)
* [Despues instalar programas varios segun gusto](#instalar-programas)
  * [Que version me conviene y en que hardware???](#que-version-me-conviene-para-que-hardware)
  * [Casos de uso en distintas maquinas especificas](#instalaciones-en-distintas-maquina-especificas)
* [El Porque debo usar Alpine](#el-porque-debo-usar-alpine)
  * **[Porque no deberia usarla](#porque-no-deberia-usarla)**
* [Aclaratorias ventajas y deficiencias](#aclaratorias-ventajas-y-deficiencias)
* [Comunidad Alpine español](#comunidad)
  * [Como contribuir a esta documentacion](#como-contribuir-a-esta-documentacion)

### Inicio:

Alpine permite instalar desde disco, usb, o red, en las dos primeras usa una imagen arrancable mientras 
que en la tercera se necesita una imagen dumpeada o un Alpine desde otra maquina. Por facilidad, 
asumiremos es la primera vez y asumiremos las dos primeras:

[instalar/instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md](instalar/instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md)
instalacion ejemplo sin borrar su antiguo linux, con algunas fotos par aarranque de dos instalaciones, 
este documento tambien particiona customizado usando optimizacion y MBR del disco en vez de UEFI.

Para mejores u/o otras combinaciones de instalacion leer [instalar/README.md](instalar/README.md)

### Instalar programas

En alpine los programas se instalan desde la red, en archivos llamados "paquetes", 
rapido y simple sin tantas dependencias a diferencia de otras linux.. 

Para las recetas de escritorios, programs y demas, leer [recetas/README.md](recetas/README.md), 
Receta rapida para listado de todos los programas recomendados: [recetas/programas-esenciales-todo-en-uno.md](recetas/programas-esenciales-todo-en-uno.md)

# QUE VERSION ME CONVIENE PARA QUE HARDWARE

Alpine no es un distro de uso general, y **no ha sido probada en muchas maquinas,** 
basado en las trazas de internet, los kernel news y los alpine bugs, se construye una 
muy acertada y obvia tabla de soporte y que soluciones (versiones de alpine) emplear.

Para mayor informacion consultar la tabla de hardware y alpines recomendados:
* [informes/hardware-y-versiones-alpine-recomendados.md](informes/hardware-y-versiones-alpine-recomendados.md)

> No pero yo tengo Alpine instalado en mi Pemtium 4 y funciona bien

Falso: desde kernel 4 el soporte de video intel Opengl quedo deprecated para estos chipset (i810/i845) 
la solucion es instalar Alpine 3.2 que tiene kernel 3.X y aun soporte bien maquinas viejas.

## Instalaciones en distintas Maquinas Especificas

Recetas especificas para marcas o modelos de computadores, HP, DELL, IBM, Intel classmates/canaimitas, 
consultelo en su index, no es como debian pero hay bastantes:

* [informes/README.md](informes/README.md)

# El Porque debo usar Alpine

1. Porque malditamente es rapida, quieres creer vas rapido o quieres ir en verdad rapido?
2. Porque la distro que usas esta llena de malditos windosers que terminaron de ponerla lenta.. 
2. Porque es ideal para instalarle cosas que consumen mucho pero al ser rapida ejecutaran muy rapido..

Sep, es **como volver a la prehistoria, rustica, pero simple, rudimentaria pero rapida.. que se le va hacer.**

## Porque no deberia usarla

* Nada profesional sirve, ni `nomachine`, **ni `oracle`**, ni `anydesk`, pero si eclipse y netbeans...
* Porque es **innecesariamente complicada, no intuitiva, y retrograda en un mundo moderno..** y con creses!
* Y porque seguro **algunos windosers ignorate gustan hacer las cosas con dos click en linux**..

**IMPORTANTE** esta ultima es importante, es un requisito imprescindible para ser windoser. Sin 
embargo las dos primeras ya coartan demasiado esta "¿distro?" perdon.. proyecto opensource...

# Comunidad

* Lista de correo: https://groups.google.com/forum/m/#!forum/venenuxsarisari
* Indice de paquetes "bien hechos": (WIP), lo siento eso de "comunity" tiene muucha basura...
* IRC chat: #venenux pero ojo debes tener paciencia... mas activa es la lista de correo

## Como contribuir a esta documentacion

Debe usar la plataforma "gitlab", y el flujo comun de pull request aqui: https://gitlab.com/mckayemu/alpineinstalls
Requisito indispensable no usar guindows ni winbuntu.-

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
2. **El boot manager es syslinux hasta la 3.8** esto al fin lo mejoraron en 3.9, syslinux es mas 
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

