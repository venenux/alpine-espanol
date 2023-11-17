# alpine-espanol

**Sitio en español para la experiencia usuario con alpine.**  https://venenux.github.io/alpine-espanol/

> Es **rustica, pero simple, rudimentaria pero rapida..** aunqeu se sienta como volver a la prehistoria..

## Indice

* [Inicio aqui: instalar alpine con varias recetas](#inicio)
* [Despues, instalar programas y recetas](#instalar-programas-y-recetas)
  * [Que version me conviene y en que hardware???](#que-version-me-conviene-para-que-hardware)
* [El Porque debo usar Alpine](#el-porque-debo-usar-alpine)
  * [Porque no deberia usarla](#porque-no-deberia-usarla)
* [Aclaratorias ventajas y deficiencias](#aclaratorias-ventajas-y-deficiencias)
* [Comunidad Alpine español](#comunidad)
  * [Como contribuir a esta documentacion](#como-contribuir-a-esta-documentacion)

# Inicio

Alpine permite instalar desde disco, usb, o red, en las dos primeras usa 
una imagen arrancable mientras que en la tercera se necesita una clonacion
dumpeada o un Alpine desde otra maquina. Por facilidad, se recomienda las 
primeras dado son las mas conocidas y faciles de entender.

En el documento [instalar/instalar-desde-imagen-a-virtualbox-alpinesolo-computadora.md](instalar/instalar-desde-imagen-a-virtualbox-alpinesolo-computadora.md)
se emplea una maquina virtual, ideal para su computador si quiere probar.

Para mejores u/o **otras combinaciones de instalacion leer [instalar/README.md](instalar/README.md)**

### A que se parece alpine linux?

| a que se parece | como usuario        | como desarollador   | como linuxero        |
| --------------- | ------------------- | ------------------- | -------------------- |
| a un linux:     |     geento          |      archlinux      |        Devuan        |
| porque es:      | dificil y tecnico   | anda a la moda      | hay que saber que se hace |

## Instalar programas y recetas

En alpine los programas se instalan desde la red, y son llamados "paquetes", 
y dependen del entorno de uso que puede ser [consola de solo letras con el documento recetas/alpine-recetas-configuracion-y-paquetes-sistema.md](recetas/alpine-recetas-configuracion-y-paquetes-sistema.md) 
o puede usar [los graficos y escritorio de trabajo con el documento recetas/alpine-recetas-configuracion-entorno-grafico-y-escritorio.md](recetas/alpine-recetas-configuracion-entorno-grafico-y-escritorio.md)

Si tiene dudas para los iniciados lease [recetas/alpine-recetas-configuracion-y-paquetes-sistema.md](alpine-recetas-configuracion-y-paquetes-sistema.md)

## QUE VERSION ME CONVIENE PARA QUE HARDWARE

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

Es importante que **para algunos que se creen expertos: linux Alpine es para machos 
y les dara una bofetada**, sin embargo difiere y tiene algunas ventajas 
como desventajas asi como cambios respectos otras distros.

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

1. **La libreria central de C es musc libc** es la principal diferencia, 
si bien permite mucha mayor rapidez tambien es un dolor de cabeza, ya que 
imposibilita el ejecutar otros programs asi como software binario en totalildad.
2. **El boot manager es syslinux** para BIOS y grub solo si detecta UEFI, 
no hay documentacion de como usar grub especifica para el sistema, 
solo la que estos documentos en español ofrecemos aqui.
3. **Estable pero casi rolling release** se limita a usar lo mas nuevo, 
y remueve sin estimar si afecta software de terceros, tal como se removio gtkglext 
despoticamente, mientras mas nuevo menos soporte tiene su hardware.

# Contactos

Consulte [documentos/navfooter.md](documentos/navfooter.md) para mas informacion.

## Como contribuir a esta documentacion

Debe usar la plataforma "codeberg", y el flujo comun de pull request aqui: https://codeberg.org/venenux/alpine-espanols
Requisito indispensable no usar guindows ni winbuntu.-

