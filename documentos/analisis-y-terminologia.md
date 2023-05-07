# FUNDAMENTOS Y TERMINOLOGIA

Hay teorías que dicen que los 2 más grandes problemas de la Ciencia de la
Computación son invalidar un caché, nombrar a las cosas, y los errores de 1 bit. Es
una broma, porque esas son 3 cosas y no dos. A eso se refiere los errores de 1 bit.

Este documento es requisito inicial para enterder la documentacion de servidores en [README.md](README.md) 
debe leerse y entenderse si aun no tiene fuertes conocimientos informaticos ni matematicos, 
despues de tener nociones elementales aqui listadas puede dirigirse a leer el [README.md](README.md).

- [COMPUTO, CAPACIDAD DE AUTOMATIZACION Y CALCULO MATEMATICO](#computo-capacidad-de-automatizacion-y-calculo-matematico)
    - [COMPUTADOR, COMPUTACION](#computador-computacion)
    - [SISTEMA, SISTEMAS Y PROCESOS](#sistema-sistemas-y-procesos)
    - [INFORMATICA](#informatica)
    - [SISTEMA INFORMATICO](#sistema-informatico)
    - [TIC o TI tecnologia de informacion](#tic-o-ti-tecnologia-de-informacion)
    - [Departamentos de TIC](#departamentos-de-tic)
    - [Tecnologia](#tecnologia)
      - [Elementos de el proceso tecnologico](#elementos-de-el-proceso-tecnologico)
      - [Innovacion](#innovacion)
      - [Sistema](#sistema)
      - [Componentes de sistemas](#componentes-de-sistemas)
        - [Ley de conservacion de materia en sistemas](#ley-de-conservacion-de-materia-en-sistemas)
      - [TIC o TI tecnologia de informacion](#tic-o-ti-tecnologia-de-informacion)
      - [Departamentos de TIC](#departamentos-de-tic)
    - [Programa](#programa)
      - [Hardware](#hardware)
      - [Software](#software)
      - [Programacion](#programacion)
      - [Sistema Operativo](#sistema-operativo)
    - [EFI y proceso de arranque de computador](#efi-y-proceso-de-arranque-de-computador)
      - [BIOS](#bios)
      - [UEFI y Boot](#uefi-y-boot)
        - [Secure Boot](#secure-boot)
- [REDES E INTERNET](#redes-e-internet)
    - [Redes](#redes)
    - [Internet](#internet)
    - [NIC](#nic)
    - [Direcciones MAC](#direcciones-mac)
    - [Direcciones IP](#direcciones-ip)
        - [Paquete de red vs Trama de red](#paquete-de-red-vs-trama-de-red)
        - [Conexión a internet desde la oficina](#conexión-a-internet-desde-la-oficina)
        - [Subredes](#subredes)
    - [Tipos de direcciones IP y subredes](#tipos-de-direcciones-ip-y-subredes)
        - [1. Direcciones IP públicas](#1.-direcciones-ip-públicas)
        - [2. Subredes IP privadas](#2.-subredes-ip-privadas)
    - [NAT](#nat)
    - [Protocolos, TCP, UDP, otros](#nat)
    - [Conexiones TCP](#conexiones-tcp)
    - [Puertos](#puertos)
    - [IP dinamica e IP estatica](#ip-dinamica-e-ip-estatica)
    - [DHCP](#dhcp)
    - [Puerta de enlace o gateway o pasarela](#puerta-de-enlace-o-gateway-o-pasarela)
    - [Firewall o Cortafuegos](#firewall-o-cortafuegos)
    - [DNS](#dns)
        - [Nombres DNS](#nombres-dns)
        - [EDNS](#edns)
        - [DNSSEC](#dnssec)
        - [archivo hosts](#archivo-hosts)
        - [DNS autoritativos, recursivos y clientes](#dns-autoritativos,-recursivos-y-clientes)
        - [Tipos de servidores DNS](#tipos-de-servidores-dns)
    - [PXE el Arranque de red o booteo de red](#pxe-el-arranque-de-red-o-booteo-de-red)
        - [Inicios de PXE](#inicios-de-pxe)
        - [Activar PXE](#activar-pxe)
        - [Proceso PXE](#proceso-pxe)
      - [Elementos PXE](#elementos-pxe)
        - [NBP](#nbp)
        - [TFTP](#tftp)
        - [DHCP](#dhcp)
        - [HTTPS y NFS](#https-y-nfs)
    - [ISP Proveedores de internet](#isp-proveedores-de-internet)
      - [Tipos de ISP](#tipos-de-isp)
        - [Conexciones ISP](#conexciones-isp)
        - [Protocolos ISP](#protocolos-isp)
    - [DSN](#dsn)
      - [AS](#as)
        - [BGP](#bgp)
        - [EBGP y IBGP](#ebgp-y-ibgp)
        - [PVP](#pvp)
- [Documentos a seguir](#documentos-a-seguir)

## COMPUTO, CAPACIDAD DE AUTOMATIZACION Y CALCULO MATEMATICO

**Computo: Capacidad de realizacion de calculo**, meramente humano ya que el calculo es
relizado por y para el ser humano. 

* **La matematica es la ciencia que permite el computo**.
* **El computo es la ciencia que permite la automatizacion**.
* **La automatizacion es la ciencia que compacta muchos computos conjuntos**.
* **Los procesos son etapas automatizadas para un objetivo funcional.**

Todos estos permiten resumir o compactar varias etapas de uno o varios procesos
para un resultado especifico en un solo proceso, para elllo se emplean 
los sistemas. La automatizacion es la ciencia que lleva a requerir recursos 
y mecanismos, sean electronicos que tengan la capacidad de computo, esto 
ultimo es un computador.

### COMPUTADOR, COMPUTACION

**Computador es la entidad capacitada de realizar computos** (sea una maquina,
mecanismo o humano), empleando la logica, sistemas e informacion, los tres elementos
pricipales para realizar el computo.

**La computacion es la ciencia del computo**, termino es mal empleado debido
a las tendencias de la misma, ya que la principal herramienta para el computo 
hoy dia es el computador electronico y los mecanismos electronicos. 

**La computadora es una entidad que realiza computos y procesos**, 
ya que es capaz de realizar varias tareas de computos distintas, las cuales 
sirven para finalidades mas complejas y distintas procedientes de computos, 
por ejemplo, dibujo asistido vectorial CAD el cual emplea muchas operaciones de
coma flotante (numeros con decimales). La computadora es una 
herramienta/entidad capaz de hacer de computador, y no esta definida 
como un aparato electronico, sino como algo que realiza computos.

### SISTEMA, SISTEMAS Y PROCESOS

Un proceso es una etapa de automatizacion de datos de informacion, para lograr 

Un sistema es la integridad de los procesos para un medio determinado. La matematica
esta ligada a sistemas, pues **la logica es parte elemental y fundamental de los sistemas**.
Entonces un sistema **es logica aplicada para realizar procesos** que llevan a un determinado fin.

### INFORMATICA

La informática es la disciplina que estudia el tratamiento automático de la información, 
**no significa trabajar con computadoras, sino automatizacion de informacion.**

Informática es un vocablo proveniente del francés informatique, acuñado por el ingeniero
Philippe Dreyfus en 1962, acrónimo de las palabras information y automatique.
En informática confluyen muchas de las técnicas que el hombre ha desarrollado a lo largo
de la historia para apoyar y potenciar sus capacidades de memoria, de pensamiento y de
comunicación. Hoy dia se confunde la informatica con sistemas informaticos.
Esta definicion puntual y sencilla, deja en claro que no tiene que ver nada con mecanica,
electronica ni computacion. La informatica utiliza recursos para automatizar el proceso y poder
proveer de facilidad de acceso a la informacion, potenciando la veracidad y disponibilidad. De
alli se emplean varios mecanismos de automatizacion, en principios estos mecanismos eran
simplemente procesos humanos que encaminaban las tareas, a medida que pasaba la historia
se empleo diversos mecanismos, primero mecanicos, poco despues electronicos, usando hoy
dia mecanismos computacionales, que son los que combinan la electronica con una logica de
proceso, esto se le llama sistema conmputacional o sistema informatico, termino mal usado
hoy dia como informatica.

### SISTEMA INFORMATICO

Se entiende por sistema informático a **la unión sinérgica del cómputo, soporte y 
comunicaciones para realizar informatica**. 

**La interrelacion de elementos especificos para proveer informacion**. En un 
sistema informatico, convergen los fundamentos de las ciencias de
la computación o calculo, la programación o metodologías, así como electrónica o mecanica y
una entidad de soporte que en la mayoria de los casos es un ser humano.
Hoy dia esto deriva en los sistemas informaticos computacionales debido a que a travez
de la computacion de calculo y el computador electronico se potencia los sistemas
informaticos.

## Tecnologia

Las técnica, métodos y procesos utilizados para lograr un objetivo. La tecnologia 
nacio con la invencion de la rueda y no con la invencion del fuego, el fuego permitio 
la tecnologia, la tecnologia permitio el avance y sin avance tecnologico la 
inteligencia se estanca.

La forma más simple de tecnología es el desarrollo y uso de herramientas básicas.

### Elementos de el proceso tecnologico

sin embargo hoy dia esto se confunde con mediocridad, hacer una herramienta que 
ayude no significa que sea la mejor forma de solucionar un problema. Por eso 
existen los dos elementos tecnologicos que permiten verdaderos resultados:

* Innovacion
* Sistemas

### Innovacion

Innovación es cuando se crean novedades, cosas nuevas, para ayudar a mejorar o avanzar 
la tecnologia, se refiere a modificar elementos o crear elementos nuevos mejorados.
Es el primer elemento clave de la tecnologia y se fundamenta en ideas logicas y no ideas vagas.

lo que significa que su avance desmedido desvirtua y deprecia los objetivos, esto es 
normal porque el avance implica desechar aquello que ya no es necesario.

Como ejemplo el mismo ser humano es un elemento desechable si en un proceso no 
cumple mas un objetivo util y beneficioso para el proceso. Esto ocurre mas rapido 
con aparatos, objetos o procesos.

Las innovaciones influyen en los valores de cada sociedad y cuestiones éticas de 
la tecnología. Los ejemplos incluyen el surgimiento de la noción de eficiencia 
en términos de productividad humana (ejemplo un robot seria superior para trabajar 
pero no para amar).

### Sistema

Un sistema es un conjunto de elementos interrelacionados entre si para lograr un objetivo. 
Es el segundo elemento clave de la tecnologia resultado de la innovacion verdadera.

### Componentes de sistemas

Los componentes de sistema son:

* **Entradas**: Datos, información, insumos que ingresan al sistema.
* **Procesos**: Cambios que se producen a las entradas para generar salidas, resultados del sistema.
* **Salidas**: Resultados de los procesos realizados en el sistema.
* **Desecho**: Resultado de transformacion de las entradas por el proceso, que no se quiere en la salida.

**CUIDADO** en toda accion de la vida real se cumple un proceso, puesto como ejemplo 
al respirar inhalamos aire pero no expulsamos aire, sino dioxido de carbono (desecho), 
la transformacion de la entrada (aire) es llevada a los globulos rojos (salida).

#### Ley de conservacion de materia en sistemas

Todos lso procesos producen desecho, nunca la misma cantidad de entrada produce la misma salida.

### TIC o TI tecnologia de informacion

Son hoy dia las tecnologías de la idiotez y las comunicaciones (TIC) es el termino dado que: 
**estas tecnologias permiten que tareas que requieren de destreza sean realizadas sin destreza**.

El termino IT se refiere a la implementacion de la informatica mediate las tecnologias, 
lo que significa que es la integración de las telecomunicaciones y las computadoras (hardware y software)
para que los usuarios puedan manipular información.

### Departamentos de TIC

* Departamento de IT: el departamento de tecnologia e informacion se encarga de que 
esten disponibles estas tecnologias, que las funciones sean realizadas empleando tecnologia automatica.
* Departamento de soporte: este se encarga de soluciones rapidas, a las fallas que presenten 
estos sistemas de IT, pero tambien deben aprender de los resultados de los cambios para solventar estas.
* Departamento de sistemas: se encarga de llevar a cabo las ideas de IT, y una vez desarrolladas 
proveer de informacion a los departamentos de soporte para que asi puedan solventar futuros problemas.

## Programa

Son reglas o conjunto de reglas para cumplir un objetivo. En computacion, estas 
reglas son interpretadas por el dispositivo.

### Hardware

Dentro de los sistemas tecnologicos es el nombre formal de la parte fisica en 
un sistema computacional.

### Software

Es el conjunto de programas que provee un proceso para un objetivo, por lo que es una 
expresion de tecnologia aplicada a el hardware.

### Programacion

Es el arte de darle logica al hardware creando software. Se puede decir que programar 
es crear software para que el hardware funcione.

### Sistema Operativo

Un sistema operativo (SO) es el conjunto de programas de un sistema informático que 
gestiona los recursos de hardware y provee servicios a los programas de aplicación de software. 
Estos programas se ejecutan en modo privilegiado respecto de los restantes.

## EFI y proceso de arranque de computador

Primera implementacion de la interfaz de firmware extensible, al mejorarse 
evoluciono en UEFI hoy dia sinónimo de UEFI.

Este nacio al crear un CPU que no tenia soporte apra arquitecturas viejas, 
ya que desde mucho tiempo los cpu aun internamente eran 16bits hasta 2021.

- [EFI y proceso de arranque de computador](#efi-y-proceso-de-arranque-de-computador)
    - [BIOS](#bios)
    - [UEFI y Boot](#uefi-y-boot)
        - [Secure Boot](#secure-boot)
        - [PE](#pe)
        - [ESP](#esp)
        - [TPM](#tpm)
        - [PCR](#pcr)
        - [SRK](#srk)
        - [UKI](#uki)
        - [Measurement](#measurement)
        - [Measurement Boot](#measurement-boot)
        - [initrd](#initrd)
        - [Shim](#shim)
        - [HSM](#hsm)
        - [DEK](#dek)
        - [LUKS2](#luks2)


### BIOS

Sistema basico de ingreso y salida, (Basic Input Output System) fue una 
estandarizacion para que todas las computadoras arranquen sistemas operativos, 
de esta manera las empresas no tenian que preocuparse por implementar 
por si mismas el arranque en los computadores, bajando los costes.

El BIOS es un sistema operativo mini dentro de cada computador. Que solo 
tiene indicaciones de que trae este computador para reportarlo al 
sistema operativo al desplegarse y asi este encuentre todo lo que manejara.

Este sistema es de 16 bits y requeire que el CPU tenga soporte 16 bits, 
lo que significa que todos los CPU en los computadores aun soportan tanto 
el modo 16bit como el 32bit, y por eso aun era posible instalar un sistema viejo.

Con la creacion del Itanium 64bit al no tener ni 32bit ni 16bit este BIOS 
no estaba presenta y se creo el EFI para el arranque de los sistemas operativos.

### UEFI y Boot

Interfaz de firmware extensible unificada", (Unified Extensible Firmware Interface)
es un estándar forzosamente adoptado para el firmware de computadores.

Su intencion fue la inclusion de soporte nativo para "SecureBoot" y "Measured Boot", 
y asi frenar las remociones de sistemas operativos pagos frente los software libre.

#### Secure Boot

Un mecanismo en el que todos los componentes de software involucrados en 
el proceso de arranque se firman criptográficamente y se comparan con un 
conjunto de claves públicas almacenadas en el hardware de la placa base, 
implementadas en el firmware, antes de su uso.

Es un sistema polemico porque estas llaves publicas firmadas estan bajo 
el control de una sola empresa (Microsoft) lo que significa un monopolio 
e intenciones de no dejar cambiar el Sistema operativo que venden, 
impidiendo la libre competencia y la innovacion tecnologica.

#### PE

Ejecutable portátil; un formato de archivo para binarios ejecutables, 
originario del mundo de Win16, pero también utilizado por el firmware UEFI.

Por conveniencia de Microsoft al no tener la tecnologia para implementar 
otros sistemas mas avanzados este formato fue incluido ala fuerza y por 
ello es otra razon de porque EFI se desecho y se creo UEFI para que el sistema 
de Windo pusiera ejecutar al carecer de tecnologia adecuada.

Los archivos PE pueden contener código y datos, categorizados en "secciones" 
etiquetadas

#### ESP

Partición del sistema EFI (EFI Special partition); una partición especial en 
un medio de almacenamiento en el que el firmware puede buscar binarios UEFI PE 
para ejecutarlos en el arranque.

#### TPM

Modulo de plataforma confiable (trusted platform module); un chip de seguridad que se 
encuentra en muchos sistemas modernos, tanto en sistemas físicos como, cada vez más, 
en entornos virtualizados.

Tradicionalmente, un chip discreto en la placa base, pero hoy en día a menudo se 
implementa en el firmware y, últimamente, directamente en el SoC de la CPU.

#### PCR

Registro de Configuración de la Plataforma (Platform Configuration Register); 
un conjunto de registros en un TPM que se inicializan a cero en el arranque. 

Los datos proporcionados primero se cifran criptográficamente. El valor hash resultante 
se combina luego con el valor anterior de la PCR y la combinación se vuelve a 
convertir en hash. El resultado se convertirá en el nuevo valor del PCR.
(siempre con los datos que se usarán a continuación durante el proceso de arranque).

#### SRK

La clave raíz de almacenamiento; (Storage Root Key) una clave criptográfica especial 
generada por un TPM que nunca abandona el TPM y se puede usar para cifrar/descifrar 
los datos pasados al TPM.

#### UKI

Imagen de kernel unificada; (Unified Kernel Image) Una combinación del 
kernel, initrd y otros recursos creada por linux para todos los sistemas Unix similares.

#### Measurement

El acto de “extender” un PCR con algún objeto de datos.

#### Measurement Boot

Un proceso de arranque en el que cada componente mide (es decir, aplica un hash 
y se extiende a una PCR de TPM, consulte más arriba) el siguiente componente 
al que pasará el control antes de hacerlo. Esto tiene dos propósitos: se puede 
usar para vincular la política de seguridad de los secretos cifrados a los 
valores de PCR resultantes (o las firmas de los mismos, consulte más arriba), 
y se puede usar para razonar sobre el software usado después del hecho, 
por ejemplo, con el fin de atestación a distancia.

#### initrd

Abreviatura de "disco RAM inicial" (initial ram disk), que, estrictamente hablando, 
es un nombre inapropiado hoy en día, porque ya no se trata de un disco RAM, 
sino de una instancia del sistema de archivos tmpfs.

También conocido como "initramfs", que también es engañoso, dado que el sistema 
de archivos ya no es ramfs, sino tmpfs (ambos son sistemas de archivos en memoria 
en Linux/Unix/Mac, con semántica diferente). El initrd se pasa al kernel y es 
básicamente un árbol del sistema de archivos en el archivo compreso (cpio/lzma).

El núcleo (kernel) desempaqueta la imagen en un tmpfs (es decir, en un sistema 
de archivos en memoria) y luego ejecuta un binario a partir de él. Por lo tanto, 
contiene los archivos binarios para el primer código de espacio de usuario 
que invoca el núcleo. Por lo general, el trabajo de initrd es encontrar el 
sistema de archivos raíz real, desbloquearlo (si está encriptado) y hacer 
la transición a él.

#### Shim

Un componente de arranque que se origina en el mundo Linux, que en cierto modo 
amplía la base de datos de claves públicas que mantiene SecureBoot (que está bajo 
el control de Microsoft) con una segunda capa (que está bajo el control de las 
distribuciones de Linux y del propietario del dispositivo físico).

#### HSM

Módulo de seguridad de hardware (hardware security module); una pieza de hardware 
que puede generar y almacenar claves criptográficas secretas y ejecutar operaciones 
con ellas, sin que las claves abandonen el hardware (aunque esto es configurable).

Los TPM pueden actuar como HSM.

#### DEK

Clave de cifrado de disco (Disk Encryption Key); una clave criptográfica asimétrica 
utilizada para desbloquear el cifrado de disco, es decir, pasada a LUKS/dm-crypt 
para activar un volumen de almacenamiento cifrado.

#### LUKS2

Configuración de clave unificada de Linux, versión 2; una especificación para un 
superbloque para volúmenes cifrados ampliamente utilizada en Linux. LUKS2 es el 
formato en disco predeterminado para el conjunto de herramientas cryptsetup. 
Proporciona una gestión de claves flexible con múltiples ranuras de claves 
independientes y permite incrustar metadatos arbitrarios en un formato JSON 
en el superbloque.


- [COMPUTO, CAPACIDAD DE AUTOMATIZACION Y CALCULO MATEMATICO](#computo--capacidad-de-automatizacion-y-calculo-matematico)
    - [COMPUTADOR, COMPUTACION](#computador--computacion)
    - [SISTEMA, SISTEMAS Y PROCESOS](#sistema--sistemas-y-procesos)
    - [INFORMATICA](#informatica)
    - [SISTEMA INFORMATICO](#sistema-informatico)
    - [TIC o TI tecnologia de informacion](#tic-o-ti-tecnologia-de-informacion)
    - [Departamentos de TIC](#departamentos-de-tic)
    - [Tecnologia](#tecnologia)
      - [Elementos de el proceso tecnologico](#elementos-de-el-proceso-tecnologico)
      - [Innovacion](#innovacion)
      - [Sistema](#sistema)
      - [Componentes de sistemas](#componentes-de-sistemas)
        - [Ley de conservacion de materia en sistemas](#ley-de-conservacion-de-materia-en-sistemas)
      - [TIC o TI tecnologia de informacion](#tic-o-ti-tecnologia-de-informacion)
      - [Departamentos de TIC](#departamentos-de-tic)
    - [Programa](#programa)
      - [Hardware](#hardware)
      - [Software](#software)
      - [Programacion](#programacion)
      - [Sistema Operativo](#sistema-operativo)
    - [EFI y proceso de arranque de computador](#efi-y-proceso-de-arranque-de-computador)
      - [BIOS](#bios)
      - [UEFI y Boot](#uefi-y-boot)
        - [Secure Boot](#secure-boot)
        - [TPM](#tpm)
        - [Measurement](#measurement)
        - [Measurement Boot](#measurement-boot)
        - [initrd](#initrd)

- [Documentos a seguir](#documentos-a-seguir)

# REDES E INTERNET

Esta guía de redes e internet se compone de las siguientes secciones:

- [COMPUTO, CAPACIDAD DE AUTOMATIZACION Y CALCULO MATEMATICO](#computo--capacidad-de-automatizacion-y-calculo-matematico)
    - [Redes](#redes)
    - [Internet](#internet)
    - [NIC](#nic)
    - [Direcciones MAC](#direcciones-mac)
    - [Direcciones IP](#direcciones-ip)
        - [Paquete de red vs Trama de red](#paquete-de-red-vs-trama-de-red)
        - [Conexión a internet desde la oficina](#conexión-a-internet-desde-la-oficina)
        - [Subredes](#subredes)
    - [Tipos de direcciones IP y subredes](#tipos-de-direcciones-ip-y-subredes)
        - [1. Direcciones IP públicas](#1.-direcciones-ip-públicas)
        - [2. Subredes IP privadas](#2.-subredes-ip-privadas)
    - [NAT](#nat)
    - [Protocolos, TCP, UDP, otros](#nat)
    - [Conexiones TCP](#conexiones-tcp)
    - [Puertos](#puertos)
    - [IP dinamica e IP estatica](#ip-dinamica-e-ip-estatica)
    - [DHCP](#dhcp)
    - [Puerta de enlace o gateway o pasarela](#puerta-de-enlace-o-gateway-o-pasarela)
    - [Firewall o Cortafuegos](#firewall-o-cortafuegos)
    - [DNS](#dns)
        - [Nombres DNS](#nombres-dns)
        - [EDNS](#edns)
        - [DNSSEC](#dnssec)
        - [archivo hosts](#archivo-hosts)
        - [DNS autoritativos, recursivos y clientes](#dns-autoritativos,-recursivos-y-clientes)
        - [Tipos de servidores DNS](#tipos-de-servidores-dns)
    - [PXE el Arranque de red o booteo de red](#pxe-el-arranque-de-red-o-booteo-de-red)
        - [Inicios de PXE](#inicios-de-pxe)
        - [Activar PXE](#activar-pxe)
        - [Proceso PXE](#proceso-pxe)
      - [Elementos PXE](#elementos-pxe)
        - [NBP](#nbp)
        - [TFTP](#tftp)
        - [DHCP](#dhcp)
        - [HTTPS y NFS](#https-y-nfs)
    - [ISP Proveedores de internet](#isp-proveedores-de-internet)
      - [Tipos de ISP](#tipos-de-isp)
        - [Conexciones ISP](#conexciones-isp)
        - [Protocolos ISP](#protocolos-isp)
    - [DSN](#dsn)
      - [AS](#as)
        - [BGP](#bgp)
        - [EBGP y IBGP](#ebgp-y-ibgp)
        - [PVP](#pvp)

- [Documentos a seguir](#documentos-a-seguir)

**IMPORTANTE**

1. El que este conectado a la red interna no significa que este con internet.
2. El que tenga internet no significa que tenga acceso a la red interna.
3. No se guie de lso iconos graficos, estos no indican si tiene internet o red realmente!

### Redes

Una red es un conjunto de reglas para permitir que dispositivos electronicos se comuniquen.

Las computadoras tienen dispositivos que permiten comunicarse usando "paquetes" de informacion.

### Internet

La internet es una red pero de nivel mundial, es simplemente una red gigante.

### NIC

Es el nombre tecnico de una "tarjeta de interfaz" de red  (Network Interface Card, NIC).

Es un hardware, un dispositivo dentro de otro dispositivo que le permite comunicarse 
en las redes. No solo los computadores lo tienen, sino telefonos asi como robots.
Ejemplos de esto son las WIFI o los BLUETOOH ambos son dispositivos NIC pero el segundo 
es de corto alcance.

### Direcciones MAC

Es un nombre unico hexadecimal que identifica cada NIC como dispositivo de red unico, 
cada aparato que se vende tiene al igual que los seres humanos una identificacion 
y esta se me denomina MAC que significa control de acceso de medios (Media Access Control)

### Direcciones IP

Cada computadora conectada a la Red o a Internet tiene un Protocolo de Internet, que 
se identifica con lo que se llama o una dirección IP que identifica la computadora en Internet. 
Hay dos tipos, IPv4 e IPv6, se expresan en la forma `nnn.nnn.nnn.nnn` las IPv4 donde 
cada `nnn` es un número entre 0 y 255; las IPv6 son `xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx` 
donde cada `xxxx` es una numeracion de 0 a 9 y despues de A a F, es decir son hexadecimales.

Cuando se conecta a un servidor web para explorar una página web, el software 
en su máquina traduce automáticamente el "nombre" del servidor web, a una dirección IP. 
Esta dirección se utiliza para conectarse al servidor web real.

#### Paquete de red vs Trama de red

La informacion de red se transmite por paquetes, paquete de datos es cada uno de los bloques 
en que se divide la información para enviar.

La informacion a enviar se divide en tramos pequeños de un tamaño conocido por las partes comunicadas, 
para simplifica el control de la comunicación, las comprobaciones de errores, la gestión 
de los equipos involucrados, etcétera.

En redes, una trama es una unidad de envío de datos. Es una serie sucesiva de bits, 
organizados en forma cíclica, que transportan información y que permiten en la recepción 
extraer esta información.

Tanto las tramas como los paquetes tienen una formacion o estructura, estan formados 
por una cabecera, la parte de los datos y una cola.

Es como un documento de texto que tiene toda esta informacion ordenada de esa manera: 
en la cabecera la identificacion; en la cola, si la hubiere, 
los mecanismo de comprobación de errores.

#### Conexión a internet desde la oficina

En un entorno de oficina, la computadora esta conectada a una subred en uno de 
los rangos de direcciones privadas. Esto significa que su computadora tendrá una 
dirección IP no única en Internet, por lo que no puede comunicarse directamente 
con otras computadoras en Internet.

#### Subredes

Este artilugio se creo debido a que asi se podria crear jerarquias en las redes. 
Ninguna computadora está conectada directamente a cualquier otra computadora en Internet. 
En cambio, cada computadora es miembro de una o más subredes. Las subredes, a su vez, 
están conectadas entre sí por máquinas llamadas enrutadores o puertas de enlace, 
que pertenecen a varias subredes, reenvían el tráfico de Internet de una subred a la otra y viceversa.

Una subred es un grupo de direcciones IP consecutivas, ejemplo: IP desde 11.22.33.0 a 11.22.33.255.

### Tipos de direcciones IP y subredes

Hay tres tipos principales de direcciones IP (o subredes) que debe conocer.

##### 1. Direcciones IP públicas

La mayoría de las direcciones IP en el rango de direcciones IPv4 
tienen el propósito de identificar de manera única una computadora en Internet. 
La dirección IP 207.155.248.18, por ejemplo, es una dirección IP pública que en 
algún momento identificó de forma única uno de los servidores que alojan el sitio web.

Cuando se esta conectado directamente esto se le llama IP publica, pero en el mundo 
no hay suficientes direcciones IP para todas las computadores asi que para esto 
estan las subredes.

##### 2. Subredes IP privadas

Se han reservado rangos especiales del rango de direcciones IPv4 
para su uso en redes privadas, donde las computadoras en dicha red no necesitan ser accesibles 
directamente desde Internet como servidores pero pueden acceder a Internet a través 
de una puerta de enlace, como clientes. Estos rangos incluyen:

* 10.0.0.0/8 (direcciones de 10.0.0.0 a 10.255.255.255)
* 172.16.0.0/12 (direcciones de 172.16.0.0 a 172.31.255.255)
* 192.168.0.0/16 (direcciones de 192.168.0.0 a 192.168.255.255)
* 127.0.0.0/8 (direcciones desde 127.0.0.0 a 127.255.255.255).

### NAT

Para comunicar las distintas redes se emplea la traducción de direcciones de red, 
también llamado enmascaramiento de IP o NAT (del inglés Network Address Translation), 

Es un mecanismo utilizado para cambiar informacion entre dos redes, 
ya que como las direcciones no pertenecen a la misma red (subred) que asignan mutuamente 
direcciones incompatibles. Consiste en convertir, en tiempo real, 
las direcciones utilizadas en los paquetes de informacion transportados. 

También es necesario editar los paquetes para permitir la operación de protocolos 
que incluyen información de direcciones dentro de la conversación del protocolo.

### Protocolos, TCP, UDP, otros

El Protocolo de Internet en sí es un protocolo relativamente rudimentario que proporciona 
solo la capacidad de entregar pequeños fragmentos de datos a otras computadoras. 
El Protocolo de Internet no proporciona confiabilidad: se pueden perder fragmentos 
de datos que se envían utilizando el Protocolo de Internet. También pueden llegar 
en un orden diferente al orden en que se enviaron los fragmentos.

Para algunos tipos de transferencia de datos, la (des) confiabilidad que ofrece 
el Protocolo de Internet está bien. 

**UDP**: El Protocolo de datagramas de usuario , o UDP, es un protocolo simple en capas 
para fines tales como retransmitir secuencias de video y audio, así como para juegos en red; 
entornos donde la respuesta y la rapidez son más importantes que la fiabilidad. 
Al transmitir video, no importa si se pierden fragmentos que componen cuadros 
intermedios del video. Lo que importa es que la mayoría de los datos llegan relativamente 
rápido, lo que permite que el video se reproduzca con una calidad razonable y sobre la marcha.

**TCP**: El Protocolo de Control de Transmisióno, o TCP, es un protocolo complejo en capas 
que contiene mecanismos para garantizar que los datos se reciban en orden y que, 
si se pierden fragmentos, se vuelven a enviar. Antes de que se pueda enviar 
cualquier información utilizando TCP, las dos computadoras deben participar en 
un breve recorrido de ida y vuelta para establecer una conexión TCP. Si se pierden 
datos durante la transmisión, la entrega de los datos subsiguientes espera hasta que 
los datos que se perdieron se retransmitan y entregan. Cuando hay una alta tasa 
de pérdida de datos en una conexión, esto puede hacer que la transmisión sea invalida.

Otros portocolos son:

* el Protocolo simple de transferencia de correo (SMTP), utilizado para la entrega de correo electrónico;
* el Protocolo de la oficina de correos (POP) y el IMAP, utilizados para la recuperación de correo electrónico;
* el Protocolo de transferencia de hipertexto (HTTP), utilizado para acceder a sitios web;
* el protocolo Secure Shell (SSH), unico para sistemas linux y unix

### Conexiones TCP

Las conexiones TCP son como llamadas telefónicas: siempre son iniciadas por una 
parte y aceptadas (o no) por la otra. La computadora que origina la conexión TCP
suele ser el cliente, y la computadora que la acepta suele ser el servidor. A veces, 
especialmente en el protocolo FTP o el de anydesk/nomachine, se establecerá 
una conexión TCP secundaria en la dirección inversa, desde el servidor hasta 
el cliente. Pero, en protocolos que no sean FTP, el cliente casi siempre inicia las conexiones.

Independientemente de la dirección en la que se establezca una conexión TCP, 
los datos siempre pueden fluir en ambos sentidos. Sin embargo, la dirección de la 
conexión TCP es importante porque determina quién es la parte iniciadora y también 
es utilizada por los componentes de la red para imponer reglas sobre si se puede 
establecer una conexión.

### Puertos

Para manejar múltiples conexiones simultáneas con la misma computadora, su computadora 
debe poder distinguirlas. Para hacerlo, a cada conexión se le asignan dos números 
de puerto, uno en cada punto final de la conexión. Luego, una conexión se identifica 
de manera única con cuatro datos: (1) dirección local, (2) puerto local, (3) dirección remota, (4) puerto remoto. 

Los números de puerto válidos están entre 1 y 65535. La parte que origina una conexión TCP 
generalmente selecciona un número de puerto local al azar. Por otro lado, el número 
de puerto de la parte que acepta la conexión debe ser conocido de antemano por la parte 
que origina la conexión.

Puertos de direcciones y servicios asociados comunes:

* 21 - FTP (conexión de control)
* 22 - SSH
* 23 - Telnet
* 25 - SMTP
* 80 - HTTP
* 110 - POP3
* 143 - IMAP4
* 443 - HTTPS
* 1080 - SOCKS
* 4000 - nomachine
* 7070 - anydesk

### IP dinamica e IP estatica

Las redes y subredes tiene un mecanismo para la asignacion de direcciones IP, cuando 
esta se coloca manual se puede decir estatica y cuadno es asignada por la red es automatica o dinamica.

* IP dinamica: se asigna una direccion distinta cada vez que el dispositivo esta en la red.
* IP estatica: se asigna una direccion igual siempre que se conecta a la red el dispositivo.

#### DHCP

El servicio que asigna las direcciones IP se le denomina DHCP que significa 
servicio de protocolo de configuracion de asignacion dinamica (Dynamic Host Configuration Protocol) 
y este se encarga de colocar direcciones IP basado en las firmas MAC de cada 
hardware de red o dispositivo de red (como el wifi, o targeta de red).
Se creo en 1985 como extensión de BOOTP, para arrancar computadores desde la red.

1. DHCP Discovery es la fase en que se realiza la solicitud de una IP con su MAC
2. DHCP Offer es la repuesta desde el servicio DHCP, donde se otorga informacion
3. DHCP Request es la respuesta SEGUN el resultado de configurar como lo ordena el DHCP
4. DHCP Acknowledge es la repuesta de redefinicion ya que nada dura para siempre en la red

Los siguientes problemas corresponden a una dirección IP que cambia continuamente.

* Cada vez que cambia la IP todas las conexiones TCP en curso hacia y desde su máquina 
se terminan y deben restablecerse utilizando la nueva IP.
* Dado que la dirección IP de su computadora es impredecible, es difícil para otros 
conectarse a ella.
* Si desea que un servidor sea accesible desde Internet desde una red interna o subred 
hay que hacer reenvío de puertos.
* Cuando la direccion publica cambia (IP dinamica publica) tambien la otra parte que 
pretende visitar este servidor desconoce esta nueva direccion IP publica.

### Puerta de enlace o gateway o pasarela

La pasarela (en inglés gateway) o puerta de enlace es un dispositivo en la red, 
y actúa de interfaz de conexión entre aparatos o dispositivos.

Se encarga de que cada comunicacion tenga un punto comun de trafico, este 
posibilita la comunicacion y compartir recursos entre dos o más ordenadores.

Un equipo que haga de puerta de enlace en una red debe tener necesariamente dos 
tarjetas de red.

En las subredes, esta la "puerta de enlace predeterminada" (default gateway) 
es la ruta predeterminada o ruta por defecto que se le asigna a un equipo y 
tiene como función enviar cualquier paquete (datos sin destino perdidos) del 
que no conozca por cuál interfaz enviarlo y no esté definido en las rutas del equipo, 
enviando el paquete por la ruta predeterminada.

En entornos domésticos, se usan los routers ADSL como puertas de enlace 
para conectar la red local doméstica con Internet; aunque esta puerta de enlace 
no conecta dos redes con protocolos diferentes, sí que hace posible conectar 
dos redes independientes haciendo uso de NAT.

### Firewall o Cortafuegos

Las computadoras modernas ejecutan una gran cantidad de servicios locales 
(como el uso compartido de archivos e impresoras) que aceptan conexiones en varios 
números de puerto, pero están destinados a ser accesibles solo desde subredes confiables 
localmente. Los firewall son un especie de filtro para el acceso.

En las organizaciones, las puertas de enlace que conectan la subred local a Internet 
generalmente cuentan con un firewall de entrada. Este firewall normalmente debe configurarse para no permitir conexiones a la subred, excepto las conexiones a servidores que deben aceptar conexiones de Internet.

En casa, su ISP generalmente no protegerá su PC del acceso malicioso desde Internet. En cambio, esta tarea debe ser realizada por un firewall instalado en el enrutador de su hogar, o si su computadora está conectada a Internet directamente, un firewall de software en su máquina. Windows XP viene equipado con dicho firewall; deberías usarlo. Las soluciones de firewall de software están disponibles para versiones anteriores de Windows.

Hay otro tipo de firewall llamado firewall de salida , o un firewall que filtra las conexiones salientes de su máquina a Internet. Generalmente es un software que intenta controlar qué programas en su máquina acceden a Internet. Esto tiene la intención de bloquear el software malicioso para que no cause demasiado daño después de que ya haya infectado su computadora. Sin embargo, el malware inteligentemente escrito puede engañar a un firewall de salida como este con engaños bastante simples y directos. Por lo tanto, la única medicina real contra el malware es evitar que infecte su computadora en primer lugar.

## DNS

Las direcciones IP son difíciles de recordar, por lo que Internet proporciona un servicio 
de traducción que traduce nombres memorables en direcciones IP asociadas. 

Esta instalación se denomina Sistema de nombres de dominio o DNS. Utiliza el DNS implícitamente 
cada vez que escribe una dirección como 'www.intranet.com': su navegador le solicita 
a su sistema operativo la traducción a una dirección IP, y el sistema operativo devuelve 
un resultado consulta con un servidor DNS definido por la conexcion de red. Este servidor 
a su vez devuelve un resultado o consulta con otro servidor DNS y asi hasta tener el resultado.

El DNS se ocupa de otro de los grandes problemas: nombrar las cosas.

#### Nombres DNS

La definición más común que encontraremos es que el DNS es la "libreta de
direcciones", o "guía telefónica" de Internet. En un comienzo el DNS permitía lo mismo: 
conectar a dos usuarios mediante la llamada de uno de ellos, al "nombre de servicio" del otro.
Desde los inicios de Internet se vió la necesidad de mantener un catálogo de nombres
de máquinas. En un comienzo las máquinas se identificaron con su "dirección IP",
pero rápidamente surgió la necesidad de asignarles un mapeo (equivalencia) a nombres que fueran
humanamente sencillos. Tanto por mejorar la "recordabilidad" o "memorización" de
ellos, como para dar una capa extra de "indirección", que permitiera por ejemplo tener
distintos servicios con distintos nombres, pero asociados al mismo servidor final, con
la misma dirección IP.

Claramente la analogía con una guía telefónica se queda corta muy pronto, pero es
un punto de partida.

#### EDNS

Podríamos decir que luego de su invención, el primer cambio importante fue la del
estándar EDNS de 1998: el “Extended DNS”, que permitió ampliar el tamaño de un
paquete DNS de los 512 bytes originales, a una cantidad teórica máxima de 64KB
(pero que en la práctica se limita a un poco más de 1KB, que es suficiente para la
cantidad de información actual). Lo más importante es que además de aumentar el
tamaño, permitió definir nuevas formas de extender los códigos de respuesta del
DNS, permitiendo el surgimiento de nuevas tecnologías como DNSSEC.

#### DNSSEC

En el caso de nuestro DNS, los vértices son "entendible por humanos" y "seguro",
utilizando DNSSEC. Pero no es descentralizado en el sentido original del papel, sino
jerárquico, una característica que aunque no cumpla con lo indicado en el papel, ha
sido vital en la práctica para mantener un orden y cumplir con requisitos legales,
comerciales y de uso.

El DNSSEC es una extension al DNS que implementa seguridad. Es decir se le añade seguridad 
a el DNS asi de simple.

Las respuestas DNS tradicionalmente no estaban firmadas criptográficamente, 
permitiendo muchas posibilidades de ataque; las extensiones de seguridad del DNS (DNSSEC) 
modifican el DNS para agregar la posibilidad de tener respuestas firmadas criptográficamente. 

DNSCurve ha sido propuesto como una alternativa a DNSSEC. Otras extensiones, 
como TSIG, agregan soporte para autenticación criptográfica entre pares de 
confianza y se usan comúnmente para autorizar transferencias de zona u operaciones 
dinámicas de actualización.

#### archivo hosts

En un comienzo de la Internet, debido a la pequeña cantidad de máquinas y
participantes, los nombres se mantuvieron en un archivo de texto simple, llamado la
"tabla de máquinas de internet" ("Internet Host Table"), conocida coloquialmente
como "hosts.txt". Todos los que se conectaban a Internet debían descargar este archivo 
desde lugares conocidos, usando el sistema de intercambio FTP. 

Hoy en dia este esta en `/etc/hosts` e incluso sistemas inferiores 
como los de Microsoft lo tienen (en el directorio system32).

#### DNS autoritativos, recursivos y clientes

De ahí se derivaron no solo los DNS autoritativos (Bind fue una de las primeras 
implementaciones de un DNS, y la más exitosa), DNS recursivos y clientes, 
sino una industria completa de nombres de dominio; una organización mundial llamada ICANN 
que es un modelo de gobernanza en Internet.

1. El servidor DNS local recibe una consulta recursiva desde el resolver del host cliente.
2. El DNS local realiza las consultas iterativas a los servidores correspondientes.
3. El servidor DNS local entrega la resolución al host que solicitó la información.
4. El resolver del host cliente entrega la respuesta a la aplicación correspondiente.

#### Tipos de servidores DNS

Estos son los tipos de servidores de acuerdo a su función:

* Primarios o maestros guardan los datos de un espacio de nombres en sus ficheros.
* Secundarios o esclavos obtienen los datos de los servidores primarios a través de una transferencia de zona.
* Locales o caché funcionan con el mismo software, pero no contienen la base de datos para la resolución de nombres.

Los servidores locales cuando se les realiza una consulta, estos a su vez consultan a los 
servidores DNS externos (primarios o secundarios), almacenando la respuesta en su base de datos 
para agilizar la repetición de estas peticiones en el futuro continuo o libre.

## PXE el Arranque de red o booteo de red

Es un metodo de arranque de sistema pero no desde el computador que enciende, 
sino que este busca en la red el sistema operativo para arrancar el mismo.

A esto se le denomina PXE o entorno de ejecucion de pre arranque.

#### Inicios de PXE

Si bien ha habido métodos anteriores para hacer que una máquina arranque 
a través de la red, PXE (Preboot eXecution Environment) es en lo que 
debe confiar si necesita hacerlo hoy. Es una especificación ampliamente 
implementada (de hecho, en todas partes y dispositivos que computen y tengan red) 
y las posibilidades de que tenga problemas con ella son muy bajas. 

#### Activar PXE

Comparado al complejo EFI o BIOS de su computadora, el arranque PXE está 
deshabilitado comunmente, solo esta como opcional. Cómo hacer que una máquina 
arranque a través de la red depende de su EFI/BIOS asi que debe averiguarlo 
por su cuenta, y no es parte de este documento, esto se habilita en el 
BIOS/UEFI de cada dispositivo computador y depende del modelo y fabricante.

#### Proceso PXE

El entorno PXE sondeará la red en busca de la información que necesita, 
necesita cargar un sistema operativo "desde la red", lo que significa 
desde otra máquina. Para poder hablar con otros hosts en la red, necesita 
formar parte de dicha red. Para esto, necesita conocer los parámetros 
de la red y requiere una dirección IP única y utilizable para sí mismo. 
Esta IP la maquina la optiene con DHCP (Dynamic Host Configuration Protocol), 
o puede ser configurada desde el mismo EFI/BIOS al arrancar con PXE.

### Elementos PXE

Lo siguiente son los elementos de software desplegados para el proceso:

* NBP program: used to deploy a boot into the target computer
* DHCP program: used to perform a way to find the NBP into the network
* TFTP server: used to offers the NBP program after find by DHCP the server
* WEB server: used to offers the rest of the files after boot the NBP program

Lo siguiente entonces son los servicios protocolos de software usado en la red:

* DHCP communication: used for IP management if not static routing
* TFTP transport: used for initial file transfering of boot files
* HTTP/NFS service: used for transfering post boot files and install files

#### NBP

El primer componente que carga es el NBP (Network Bootstrap Program) que
es la pieza de software, equivalente al estor de arranque pero desde la red.

#### TFTP

La comunicacion es usando transferencias TFTP (Trivial File Transfer Protocol), 
muy similar a FTP (File Transfer Protocol) pero mucho, mucho más primitivo y simple.

#### DHCP

Esto (requiere que) pide/tenga DHCP (Dinamic Host Configuration Protocol) ya que en caso 
contrario complicaria la configuracin y proceso ademas de que no todos los UEFI/BIOS 
permiten configurar el modo de arranque NBP (Network Bootstrap Program) y 
no todos los dispositivos de red o computadores permiten arranque por ip estatica.

#### HTTPS y NFS

El NBP se ejecuta cuando se completa la transferencia del mismo. Luego descargará su 
archivo de configuración y luego actuará en consecuencia, lo más probable es 
que obtenga un kernel del sistema operativo y archivos adicionales, 
y luego inicie el kernel del sistema operativo que se desea arrancar desde red para instalar.

HTTPS es el protocolo web, por el que se navega la internet y es usado 
para los archivos mas grandes despues que se despliega el NBP, en 
los casos de FreeBSD se emplea es NFS (network file system).

TFTP es lo que habla y usa el código de arranque PXE. Pero debido a su 
naturaleza muy simple, TFTP no es adecuado para transferencias de archivos 
más grandes. Por lo tanto, dichos archivos generalmente se cargan por 
otros medios una vez que el núcleo se ha iniciado y hay más opciones 
disponibles. A FreeBSD le gusta usar NFS, mientras que en Linux HTTP se 
usa a menudo para recibir un sistema de archivos o una imagen del medio 
de instalación y el resto de lso medios necesarios para el sistema o instalacion.

## ISP Proveedores de internet

El internet, necesita ser provisto a traves de interconexciones de red ya que 
las redes simples terminarian colapsando ante el flujo de informacion.

El proveedor de servicios de internet (ISP: Internet Service Provider) es 
la entidad que fuciona como empresa, y brinda la conexión a Internet.

### Tipos de ISP

Un ISP conecta a sus usuarios a Internet a través de diferentes tecnologías 
como ADSL, GSM, ATM, usando distintos medios como fibra óptica, satélite, etc.

#### Conexciones ISP

* Acceso Sin cable o indirecto
    * Telefónico (Diial-Up) o conexión por línea conmutada
    * Red de Telefonía Móvil o 
* Acceso cableado o directo
    * DSL (línea de abonado digital o línea de suscriptor digital, Digital Subscriber Line)
       * ADSL (Línea Digital de Suscriptor Asimétrica, Asymmetric Digital Subscriber Line)
       * HDSL (High bit rate Digital Subscriber Line, Línea de abonado digital de alta velocidad binaria)
       * ISDN (red digital de servicios integrados, Integrated services digital networking)
            * IDSL (acronico ISDN Digital Subscriber Line, DSL sobre líneas ISDN)
    * Cablemódem (CATV: Community Antenna Television)
    * Fibra Óptica (FTTH: Fiber to the Home)
    * Línea Eléctrica (BPL: Broadband Power Line)

#### Protocolos ISP

* Protocolos de acceso directo:
    * UMTS (Universal Mobile Telecommunications System)
    * HSDPA (High Speed Downlink Packet Access)
    * RDSI o La red digital de servicios integrados
* Protocolos de acceso indirecto:
    * Por Bandas de frecuencias
        * Wireless Personal Area Network (WPAN), red de área personal inalámbrica
            * Bluetooth
        * Wireless Local Area Network (WLAN), red de área local inalámbrica
            * Wifi
        * Wireless Metropolitan Area Network (WMAN), red de área metropolitana inalámbrica
            * WiMAX (Worldwide Interoperability for Microwave Access)
            * LMDS (Local Multipoint Distribution Service)
        * Wireless Wide Area Network (WWAN), red de área amplia inalámbrica
            * UMTS
            * GPRS
            * EDGE
            * CDMA y CDMA2000
            * GSM
            * CDPD
            * Mobitex
            * HSPA
            * 3G
            * 4G
            * 5G
     * Satelital (DVB-S, DVB-S2, DVB-S2X: Digital Video Broadcast - Satellital)

## DSN

La red digital de servicios integrados (RDSI) está definida por el Sector 
de Normalización de las Telecomunicaciones de la UIT (UIT-T) 
de la Unión Internacional de Telecomunicaciones (UIT).

Es la red que procede por evolución de la Red Digital Integrada (RDI) 
y que facilita conexiones digitales extremo a extremo para proporcionar 
una amplia gama de servicios, tanto de voz como de otros tipos, 
y a la que los usuarios acceden a través de un conjunto de interfaces normalizados

### AS

Cada ISP tiene sistemas internos para las comunicaciones y el internet, 
a estos sistemas se les denomina Sistemas Autonomos (Autonome Systems)

#### Interconexciones AS

Las relaciones que existen entre distintos sistemas autónomos son principalmente 
de interconexión voluntaria (peering) y de tránsito. 

* Transito: Básicamente una relación de tránsito es la que existe entre un proveedor 
y un cliente, de modo que el cliente pague por los recursos de Internet que 
le puede suministrar su proveedor. 

* Peering: Las relaciones de peering no suelen ser pagadas y consisten en 
un enlace para comunicar dos sistemas autónomos con el fin de reducir costes, 
latencia, pérdida de paquetes y obtener caminos redundantes.

#### BGP

Es una comuniacion entre los ISP, se llama puerta de enlace de frontera o BGP 
del inglés Border Gateway Protocol es un protocolo mediante el cual se intercambia 
información de encaminamiento entre sistemas autónomos. 

Cada proveedor o ISP tiene sus propios sistemas y conexciones, el protocolo BGP 
permite interconectarlos. Entre los sistemas autónomos de los ISP se intercambian 
sus tablas de rutas a través del protocolo BGP. Este intercambio de información 
de encaminamiento se hace entre los routers externos de cada ISP.

Esto significa que todos los ISP deben tener dispositivos y ser compatibles con BGP, 
es el protocolo más utilizado para redes con intención de configurar un protocolo 
de puerta de enlace exterior (EGP de Exterior Gateway Protocol) usado antiguamente.

#### EBGP y IBGP

Se debe distinguir entre BGP externo (eBGP) y BGP interno (iBGP) en función 
de si la información se intercambia dentro dentro del AS o entre dos AS distintos.

#### PVP

El protocolo BGP utiliza el protocolo de vector de caminos, (Path vector protocol) 
para el intercambio de información de encaminamiento en la red. Se transmite una lista 
con identificación de los AS por los que pasa el anuncio. De esa manera se conseguirá 
saber cómo llegar a cualquier dirección del prefijo propagado así como estar 
preparado para cursar tráfico para cualquier dirección del prefijo.

## Documentos a seguir

| documento                                         | enlace lectura documento |
| ------------------------------------------------- | ------------- |
| README (indice de los documentos)                 | [README.md](README.md) |
| Indice de este documento (fundamentos y terminos) | [FUNDAMENTOS Y TERMINOLOGIA](#fundamentos-y-terminologia) |
| Documento de datos/usuarios de los servidores     | [diagnostico-servidores-datos.md](diagnostico-servidores-datos.md) |
| Documento de troubleshooting o diagnostico        | [diagnostico-troubleshooting.md](diagnostico-troubleshooting.md) |

