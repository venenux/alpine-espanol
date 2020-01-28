https://mckayemu.github.io/alpineinstalls/

# alpineinstalls

**Sitio en español para la experiencia usuario con alpine.** 

### A que se parece alpine linux?

| a que se parece | como usuario        | como desarollador   | como linuxero        |
| --------------- | ------------------- | ------------------- | -------------------- |
| a un linux:     |     geento          |      archlinux      |        Devuan        |
| porque es:      | dificil y tecnico   | anda a la moda      | hay que saber que se hace |

Es **como volver a la prehistoria, rustica, pero simple, rudimentaria pero rapida.. hace bien lo que debe hacer.**

## Indice

* [Inicio aqui: instalar alpine con varias recetas](#inicio)
* [Despues, instalar programas y recetas](#instalar-programas-y-recetas)
  * [Que version me conviene y en que hardware???](#que-version-me-conviene-para-que-hardware)
* [El Porque debo usar Alpine](#el-porque-debo-usar-alpine)
  * [Porque no deberia usarla](#porque-no-deberia-usarla)
* [Aclaratorias ventajas y deficiencias](#aclaratorias-ventajas-y-deficiencias)
* [Comunidad Alpine español](#comunidad)
  * [Como contribuir a esta documentacion](#como-contribuir-a-esta-documentacion)

## Inicio:

Alpine permite instalar desde disco, usb, o red, en las dos primeras usa una imagen arrancable mientras 
que en la tercera se necesita una imagen dumpeada o un Alpine desde otra maquina. Por facilidad, 
asumiremos es la primera vez y asumiremos las dos primeras:

[instalar/instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md](instalar/instalar-desde-cdrom-usb-a-discoreal-dualboot-guia.md)
instalacion ejemplo sin borrar su antiguo linux, con algunas fotos para arranque de dos instalaciones, 
este documento tambien particiona customizado usando optimizacion y MBR del disco en vez de UEFI.

Para mejores u/o **otras combinaciones de instalacion leer [instalar/README.md](instalar/README.md)**

## Instalar programas y recetas

En alpine los programas se instalan desde la red, en archivos llamados "paquetes", 

Para las recetas de escritorios, programs y demas, leer [recetas/README.md](recetas/README.md), 

Receta listado de programas recomendados: [recetas/programas-esenciales-todo-en-uno.md](recetas/programas-esenciales-todo-en-uno.md)

### QUE VERSION ME CONVIENE PARA QUE HARDWARE

**Alpine no es un distro de uso general, y no ha sido probada en muchas maquinas, sin embargo:** 
basado en las trazas de internet, los kernel news y los alpine bugs, se construye una 
muy acertada y obvia tabla de soporte y que soluciones (versiones de alpine) que emplear.

Para mayor informacion consultar la tabla de hardware y alpines recomendados en cada caso:
* [informes/hardware-y-versiones-alpine-recomendados.md](informes/hardware-y-versiones-alpine-recomendados.md)

> No pero yo tengo Alpine instalado en mi Pemtium 3 y funciona bien

Falso: desde kernel 4 el soporte de video intel Opengl quedo deprecated para estos chipset (i810/i815) 
la solucion es instalar Alpine 3.2 que tiene kernel 3.X y aun soporte bien maquinas viejas, de alli 
actualizar algunos programas sin quitar el kernel.

**Es importante aclarar que Alpine es extremadamente rapida y eficiente, pero para algunas cosas 
requiere mas trabajo de lo habitual, en las siguientes secciones se explica esto a detalle:**

# El Porque debo usar Alpine

1. Porque malditamente es rapida, quieres creer vas rapido o quieres ir en verdad rapido?
2. Porque la distro que usas esta llena de malditos windosers que terminaron de ponerla lenta.. 
2. Porque es ideal para instalarle cosas que consumen mucho pero al ser rapida ejecutaran muy rapido..

Alpine linux es la distro con escritorio mas rapida del planeta y de la historia, super eficiente.

### Porque no deberia usarla

* Ningun programa profesional sirve, ni `nomachine`, ni `oracle`, etc...
* Porque es innecesariamente complicada, poco intuitiva, y retrograda en un mundo moderno.. y con creses!
* Porque eres un winloser ignorate y me gustar hacer las cosas con dos click..

**IMPORTANTE** esta ultima es importante, es un requisito imprescindible para ser windoser estupido.

# Aclaratorias, ventajas y deficiencias

Es importante que **para algunos que se creen expertos linux Alpine es para machos y les dara una bofetada**, 
sin embargo difiere y tiene algunas ventajas como desventajas asi como cambios respectos otras distros.

Tal **como lo dice al web oficial de alpine, no se enfoca sino en contenedores y dispositivos de red**, 
no es uan distro para uso comun ni diario, este sitio se enfoca en poder hacer eso documentado en español.

El problema es que Alpine linux es demasiado recortado, porque si monto un servidor no montare cafeteras con linux recortados.
La razon es obvia, si quiero un servidor monto "yerro" en un AWS, ahora para maquinas viejas 
si no arranca un escritorio es ilogico porque como haces que otra persona te ayude?.
Esto se evidencio en la lista de correo en https://lists.alpinelinux.org/~alpine/users/%3CCALci+FSzpExSO8hp8HFNP+GdReCM98_JfXMfms4BeY5BOUMRnw%40mail.gmail.com%3E#%3Cbf7c74b3aec178b3143bb6d305234dba@dereferenced.org%3E
**donde dice claramente "es para usuarios que administran" descartando multimedia o uso de oficina**, 
una tristeza y evidencia de las llimitaciones que posee esta distribucion.

Las controversias sobre si es para uso diario viene dado por las dos mas grandes diferencias, 
el uso de musc libc y el que es minimalista, teniendo en su principal repositorio solo paquetes servicios 
el que empaqueta es solo a manera de colaboracion y cuando no es experto suceden incompatibilidades.

1. **La libreria central de C es musc libc** es la principal diferencia, si bien permite 
mucha mayor rapidez tambien es un dolor de cabeza, ya que imposibilita tener otros idiomas 
configurados de manera dual ademas de imposibilitar compilar muchos programas, muchos 
no asuma que todo compilara como ud cree conocer, esto no es Winbuntu.. debera leer!.
2. **El boot manager es syslinux** por defecto, pero si tine UEFI automatico usara grub, 
syslinux es mas delicado, puesto en cada modificacion debe volver escribirse el MBR. 
en nuestros documentos no apoyaremos syslinux por ser delicado, complicar el soporte UEFI 
y exigir escribir en el MBR en cada cambio por mas minimo que sea.
3. **Para UEFI debe cambiar el bootmanager**, no se recomienda usar EFI/UEFI y desactivelo, 
el soporte grub es bueno con el UEFI en Alpine desde la version 3.8.5, pero mejor es BIOS, 
desactive el UEFI y mejor use particionamiente MBR en vez de GPT para evitar problemas. 
Hoy dia el instalador soporta bien el UEFI y automaticamente usa grub desde Alpine 3.10.3.
4. **Estilo rolling releases** asi que una nueva version de cualqueir programa, 
debe tener cuidado al actualizar, porque puede que su hardware o su configuracion deje 
de funcionar repentinamente, alpine linux tiene versiones estables, pero se limita a en 
cada nueva version usar lo mas nuevo posible pero estable, un ejemplo es que mientras mas 
nuevo menos soporte tiene su hardware viejo, como las intel pentium3 o las zoran viedo grabber.

# Comunidad

* Lista de correo: https://lists.alpinelinux.org/~alpine/users
* Grupo telegram: https://t.me/alpine_linux_espanol y el ingles: https://t.me/alpine_linux
* Chat IRC oficial: irc://irc.freenode.net/alpine-linux pero es en ingles...
* Indice de paquetes "bien hechos": (WIP), lo siento eso de "alpine comunity" tiene muuucha basura...

No usan foros web, tenia un foro pero fue descartado porque la lista de correo es similar y cumple..

## Como contribuir a esta documentacion

Debe usar la plataforma "gitlab", y el flujo comun de pull request aqui: https://gitlab.com/mckayemu/alpineinstalls
Requisito indispensable no usar guindows ni winbuntu.-

