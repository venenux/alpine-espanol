fail2ban para linux alpino
============================

Monitoree el **registro** de servicios comunes para detectar patrones en fallas de autenticación
y toma acciones configuradas basadas en esos fallos.

La wiki alpina oficial es una mierda, así que comenzamos una cuenta alpina centrada en profesionales.

> **Advertencia**: ¡debe leer la LICENCIA de este documento, al final aquí!

## Introducción

Cuando fail2ban está configurado para monitorear los registros de un servicio, mira un filtro
que se ha configurado específicamente para ese servicio. El filtro está diseñado para
identificar fallas de autenticación para ese servicio específico mediante el uso de
(desafortunadamente) expresiones regulares complejas y luego traducidas a una regla
para prohibir el origen infractor.

### Expresiones regulares

Las expresiones regulares son un lenguaje de plantillas común que se utiliza para la coincidencia de patrones.
básicamente consta de dos tipos de personajes:

* los caracteres literales regulares y
* los metacaracteres

Estos metacaracteres son los que dan el poder a las expresiones regulares.
Define estos patrones de expresión regular en una variable interna `failregex`
y básicamente para el **filtro** del **log**.

### Filtros y failregex

De forma predeterminada, Fail2ban incluye **archivos de filtro** para servicios comunes. Cuando un **registro**
de cualquier **servicio**, como un servidor web, coincide con `failregex` en su filtro, un
La **acción** predefinida se ejecuta para ese servicio.

### Acciones

La **acción** se puede configurar para hacer muchas cosas diferentes, con el valor predeterminado "prohibir"
el host/dirección IP infractora modificando las **reglas de firewall** locales.

Esas acciones se pueden ampliar para, por ejemplo, enviar un correo electrónico al administrador del sistema.

### Reintentos

De forma predeterminada, se tomarán medidas cuando se produzcan **fallos de autenticación de número**
detectado **en un período o tiempo** definido en minutos, con un valor predeterminado definido
tiempo de prohibición. Esto es configurable.

### cárceles o jails

La agrupación de **reintentos** + **acciones** + **filtros** usando **expresiones regulares**,
están **definidas como jaulas (jails)**, cada una contiene esas definiciones y será
**traducido a un conjunto de reglas para el firewall**.

Cuando se utiliza el **firewall de iptables** predeterminado, fail2ban crea un nuevo conjunto de firewall
reglas, también llamadas cadena, cuando se inicia el servicio. Añade una nueva regla a
la cadena INPUT que envía todo el tráfico TCP dirigido al puerto del servicio a
la nueva cadena. En la nueva cadena inserta una única regla que regresa a la cadena INPUT.
La cadena y las reglas asociadas se eliminan si se detiene el servicio Fail2ban.

## fail2ban alpino

Fail2ban se configura a través de varios archivos ubicados dentro de una jerarquía bajo
el directorio `/etc/fail2ban/` por defecto.

El paquete se inició desde 0.9 pero en Alpine 3.9 se incluyó un 0.10 estable.
versión de fail2ban, por lo que cualquier tutorial de cómo hacerlo será válido para cualquier versión alpina.

### paquete

| paquete alpino  | desde | Uso breve | Paquete relacionado |
| --------------- | ------ | ------------------------------- | --------------------- |
| falla2ban       | v3.8  | Archivos y servicio principal de fail2ban | monitorización whois py3-dnspython |
| fail2ban-test   | v3.14 | scripts de prueba auxiliares de fail2ban | |
| fail2ban-openrc | v3.10 | scripts de inicio y archivos de inicio | monitorización whois py3-dnspython |
| fail2ban-doc    | v3.10 | páginas de manual y archivos adicionales | mandoc de páginas de manual |
| fail2ban-pyc    | v3.18 | versión compilada de las pitones | monitor whois de fail2ban |

### Configuraciones

Las configuraciones están definidas por el script de servicio fail2ban, después de la instalación.
Necesita una base de datos backend, y también un usuario dedicado, también tiene archivos de configuración.

| Artefacto | Nombre | Por defecto o empaquetado | Personalizable |
| --------------- | ---------------- | -------------------------- | ------------ |
| Programa binario | servidor fail2ban | `/usr/bin/fail2ban-servidor` | no |
| Administrador binario | cliente fail2ban | `/usr/bin/fail2ban-cliente` | no |
| Guión demonio | falla2ban | `/etc/init.d/fail2ban` | no |
| Usuario demonio | falla2ban | `/var/lib/fail2ban/` | optar. ejecutar como root |
| Zócalo del demonio | fail2ban.sock | `/var/run/fail2ban/fail2ban.sock` | no, depende |
| Archivos de inicio de sesión | objetivo de registro | `/var/log/fail2ban.log` | no, depende |
| Archivos de base de datos | archivo db | `/var/lib/fail2ban/fail2ban.sqlite3` | no, depende |
| Directorio de configuración | N/A | `/etc/fail2ban/` | no |
| Configuración global | fail2ban.conf | `/etc/fail2ban/fail2ban.conf` | no |
| Archivo de configuración | cárcel.conf | `/etc/fail2ban/jail.conf` | no |
| Personalización | cárcel.local, *.local | `/etc/fail2ban/jail.d/` | si, depende |

### Lo que deberías saber

Los archivos de configuración predeterminados no se pueden tocar, son solo referencias y administrados por
las actualizaciones del paquete, los cambios propios del servidor pueden ser incompatibles con algunos futuros
versiones, no deberías editarlo en el lugar. En resumen:

El archivo `fail2ban.conf` configura algunas configuraciones operativas como la forma en que
daemon registra información y el archivo socket y pid que utilizará. La configuración principal,
sin embargo, se especifica en los archivos que definen las "cárceles" por aplicación.

1. No cambie `jail.conf`, en su lugar simplemente cree `jail.local` y solo coloque variables modificadas.
2. No cambie `filter.conf`; en su lugar, cree `filter.local` e inserte solo anular/añadir configuraciones.

Cualquier valor definido en `jail.local` anulará los de `jail.conf`, pero si no hay ninguno
está definido, por lo que utilizará los valores predeterminados en `jail.conf` y lo mismo para `filter.conf`.

### instalacion fail2ban alpine

* actualizar repositorios de apk
* También se incluyen paquetes instalados de fail2ban con soporte ipv6
* Se instalaron los paquetes necesarios porque los desarrolladores de Alpine son estúpidos.
* agregar para iniciar el servicio de script de inicio
* antes de configurar asegúrese de detener el servicio

```
apk update

apk add fail2ban iptables ip6tables

apk add monit whois py3-dnspython fail2ban-pyc logrotate python3

rc-update add fail2ban

/sbin/service fail2ban stop
```

### configuración de fail2ban con firewall predeterminado

* habilitar iptables con soporte ipv6 y también ipv4
* crear el archivo de configuración de la cárcel local
    * por defecto, Alpine debería usar common como Debian, como lo hace Geento.
    * no uses dns, los atacantes pueden cambiar el dominio con dns falsos
    * prohibir la dirección infractora con un tiempo de baneo de 16 minutos, luego se le quitará la prohibición
    * ventana de 10 minutos a la que prestar atención cuando se buscan atacantes repetidos
    * máximos intentos fallidos en la última ventana de "findtime" período
    * ignorar la dirección IP del loopback local
    * ignorar la dirección IP automática del servidor o máquina
    * incrementar el tiempo de prohibición si encontramos más intentos en cada nuevo intento de ataque
    * no prohibir más de seis días para evitar cambios de direcciones IP dinámicas
    * buscar en todas las cárceles, por lo que si hay múltiples servicios bajo ataque
    * use un backend de servicio que no sea de mierda, porque usamos openrc
    * codificación automática del texto de los archivos de registro para filtrar las búsquedas
    * deshabilitar todas las cárceles, solo habilitamos servicios específicos
    * use el modo normal no agresivo para prohibir acciones
    * utilizar el servidor de firewall de iptables
* iniciar el servicio
* reiniciar el servicio de registro para sincronizar todos los servicios

```
rc-update add ip6tables && rc-update add iptables 

cat > /etc/fail2ban/jail.local << EOF
[INCLUDES]
before = paths-debian.conf
[DEFAULT]
usedns               = no
bantime              = 16m
findtime             = 10m
maxretry             = 2
ignoreip             = 127.0.0.1
ignoreself           = true
bantime.increment    = true
bantime.maxtime      = 6d
bantime.overalljails = true
backend              = polling
logencoding          = auto
enabled              = false
mode                 = normal
banaction            = iptables-multiport
EOF

/sbin/service fail2ban restart

/sbin/service syslog restart || /usr/sbin/service rsyslog restart
```

## Cortafuegos y servicios

De forma predeterminada, Fail2ban usa iptables. Sin embargo, la configuración de la mayoría de los firewalls
y servicios es sencillo. Desafortunadamente, paquetes de cortafuegos en Alpine
es tan malo que solo tenemos una opción (sin personalizaciones complicadas)
entonces se basa en "ufw". Incluso shorewall/awall es una mierda en tales configuraciones.

### configurando fail2ban con la acción predeterminada de Shorewall

Cualquier Shorewall o Awall en alpine tiene problemas, y especialmente porque los 
desarrolladores no tienen un buen soporte para configuraciones exquisitas, por lo que no se recomienda.

El fail2ban upstream tampoco se comporta bien con el shorewall, consulte
https://github.com/fail2ban/fail2ban/issues/2031#issuecomment-361612869
porque **la acción del shorewall no tiene ningún "ancla", ni cadena ni mesa ni
algo parecido**.

El firewall alpine awall tiene algunas mejoras y ahora no depende del muro de costa,
pero aún no está bien integrado, por lo que evita usarlo si instalas fail2ban.

### configurando fail2ban con la acción predeterminada de firewalld

No recomendado, es una basura basada en feladora python con múltiples problemas ascendentes:

* https://github.com/fail2ban/fail2ban/issues/1609#issuecomment-303085942
* https://github.com/fail2ban/fail2ban/issues/2666#issuecomment-601620071
* https://github.com/fail2ban/fail2ban/issues/2503#issuecomment-533105500

Firewalld tiene dos servidores; iptables y nftables. En el backend de iptables
genérico "aceptar conexiones existentes" ocurre antes de las reglas agregadas mediante `--direct`
se ejecutan y, por lo tanto, las reglas agregadas para bloquear el tráfico en realidad no bloquearán
conexiones preexistentes. Cuando se agregó el backend de nftables, esto se solucionó:
las reglas agregadas con --direct siempre se ejecutan antes del genérico "aceptar
conexiones existentes".

Conclusión: ¡firewalld siempre tiene trucos predeterminados que no son administrados por fail2ban!

### configurando fail2ban con la acción predeterminada de ufw

Esto sustituirá completamente los iptables por ufw (pero aún necesitarás iptables):

* Paquetes fail2ban instalados con soporte ipv6 incluidos también ufw
* agregar para iniciar los **servicios de script de inicio en orden específico**
* antes de configurar asegúrese de detener el servicio
* habilitar iptables con soporte ipv6 y también ipv4
* crear el archivo de configuración de la cárcel local
    * por defecto, Alpine debería usar common como Debian, como lo hace Geento.
    * no uses dns, los atacantes pueden cambiar el dominio con dns falsos
    * prohibir la dirección infractora con un tiempo de baneo de 16 minutos, luego se le quitará la prohibición
    * ventana de 10 minutos a la que prestar atención cuando se buscan atacantes repetidos
    * máximos intentos fallidos en la última ventana de "findtime" período
    * ignorar la dirección IP del loopback local
    * ignorar la dirección IP automática del servidor o máquina
    * incrementar el tiempo de prohibición si encontramos más intentos en cada nuevo intento de ataque
    * no prohibir más de seis días para evitar cambios de direcciones IP dinámicas
    * buscar en todas las cárceles, por lo que si hay múltiples servicios bajo ataque
    * use un backend de servicio que no sea de mierda, porque usamos openrc
    * codificación automática del texto de los archivos de registro para filtrar las búsquedas
    * deshabilitar todas las cárceles, solo habilitamos servicios específicos
    * use el modo normal no agresivo para prohibir acciones
    * utilizar el servidor de firewall ufw
* iniciar el servicio
* reiniciar el servicio de registro para sincronizar todos los servicios

```
apk update && apk add fail2ban ufw ip6tables iptables whois py3-dnspython logrotate

rc-update add ip6tables && rc-update add iptables && rc-update add ufw && rc-update add fail2ban 

/sbin/service fail2ban stop

ufw enable

cat > /etc/fail2ban/jail.local << EOF
[INCLUDES]
before = paths-debian.conf
[DEFAULT]
usedns               = no
bantime              = 16m
findtime             = 10m
maxretry             = 2
ignoreip             = 127.0.0.1
ignoreself           = true
bantime.increment    = true
bantime.maxtime      = 6d
bantime.overalljails = true
backend              = polling
logencoding          = auto
enabled              = false
mode                 = normal
banaction            = ufw
EOF

/sbin/service fail2ban restart

/sbin/service syslog restart || /usr/sbin/service rsyslog restart
```


> **Advertencia** esta configuración no se recomienda, hay varios problemas con ella, el firewall backend basado en iptables es la solución que mejor funciona

Los desarrolladores no recomiendan la acción de firewall predeterminada para ufw,
como https://github.com/fail2ban/fail2ban/discussions/3401#discussioncomment-4075123
al final de las líneas, con más explicaciones.

Se recomienda ufw para escanear derrotas, porque ufw depende de iptables, consulte
siguiente sección para:

### configurando fail2ban con escaneo de registro ufw

Esto utilizará el registro ufw para prohibir los intentos de escaneo.

* ¡Primero configure usando el servidor de firewall predeterminado de iptables como primera sección aquí!
* Luego agregue estas configuraciones ejecutando esos comandos:

```
cat > /etc/fail2ban/filter.d/ufw-port-scan.conf << EOF
[Definition]
failregex = ^\s*\S+ kernel:(?: +\[[^\]]+\])? \[UFW (?:LIMIT )?BLOCK\] (?:\b(?:IN=\w+|OUT=|(?:(?!OUT=|IN=)[A-Z]+=[^ \[]*)+) )*SRC=<ADDR> DST=\S+
ignoreregex =
EOF

cat > /etc/fail2ban/jail.d/03-ufw-port-scan.local << EOF
[ufw-port-scan]
logpath  = /var/log/ufw.log
protocol = all
maxretry = 10
EOF

/sbin/service fail2ban restart

/sbin/service syslog restart
```

Si tiene una instalación fail2ban muy antigua, esta idea se deriva de
el sitio https://blog.fernvenue.com/archives/ufw-with-fail2ban/

## cárceles fail2ban o jails

Nosotros evitaremos la unificación de archivos por lo que la modularización proporcionará
organización sobre servicios independientes, por ejemplo podemos tener ssh y smtp
pero si desinstalamos o cambiamos smtp para que el script pueda administrar las cárceles sin
afecta el servicio ssh... así que podemos eliminar la cárcel smtp de forma independiente.

Esas cárceles dependerán sólo de los servicios habilitados, por lo que debes conocer
Expresiones regulares y filtrado. Aquí solo proporcionamos las opciones más cercanas de forma predeterminada.
configuración y cómo debes hacer cada uno, ¡usando siempre la modularización!

1. En primer lugar, habilitar cárceles por reincidencia para que los agresores sean castigados con mayor dureza.
2. siempre habilite la cárcel del servicio ssh
3. No habilite ninguna cárcel que no tenga instalado el servicio respectivo.

### cárcel de recaída

```
cat > /etc/fail2ban/jail.d/00-recidive.conf << EOF
[recidive]
enabled   = true
bantime   = 8d
findtime  = 1d
EOF

/sbin/service fail2ban restart

/sbin/service syslog restart
```

### ssh cárcel alpina

En este ejemplo perfectamente preparado para cualquier servidor profesional,
Usamos los archivos `alpine-sshd` ya empaquetados, ya proporcionados
desde la versión fail2ban 0.10 a Alpine v3.10 y mejorado en las últimas versiones.

También en este servicio agregamos un puerto 19226 personalizado al ssh interno bifurcado
servicio, porque en este ejemplo tenemos dos servicios ssh ejecutándose. Si usted
lo desea, o/y si cambió el puerto ssh, simplemente cambie "ssh" al número de puerto
y eso es todo, si tienes múltiples servicios ssh puedes definir múltiples
números de puerto. Pero si cambiaste el nombre de las secciones `sshd-key` por ejemplo,
debemos definir qué filtro se utilizará, como lo hicimos aquí:

```
cat > /etc/fail2ban/jail.d/00-sshd.local << EOF
[sshd]
enabled  = true
mode     = aggressive
filter   = alpine-sshd
port     = ssh
logpath  = /var/log/messages
maxretry = 2
maxlines = 3
[sshd-ddos]
enabled  = true
mode     = aggressive
filter   = alpine-sshd-ddos
port     = ssh
logpath  = /var/log/messages
maxretry = 3
maxlines = 4
[sshd-key]
enabled  = true
filter   = alpine-sshd-key
port     = ssh
logpath  = /var/log/messages
maxretry = 2
bantime  = 2d
EOF

/sbin/service fail2ban restart

/sbin/service syslog restart
```

> **Nota**: el error de los inicios de sesión sin contraseña se resuelve con los paquetes 0.11+

Estas configuraciones anteriores si usa la versión reciente del paquete 0.11+
aborda los siguientes casos de uso:

* intenta iniciar sesión con contraseñas incorrectas
* intentos de iniciar sesión por parte de atacantes ddos
* intenta iniciar sesión sin clave privada
* intenta iniciar sesión con una clave privada incorrecta
* Los intentos de iniciar sesión con una contraseña incorrecta no se registran

### Cárcel PAM para locales

> **Advertencia**: las últimas versiones deben usar los paquetes `shadow-login` o `util-linux-login`

```
apk add shadow linux-pam

cat > /etc/fail2ban/jail.d/01-pam.local << EOF
[pam-all]
enabled  = true
mode     = normal
filter   = pam-generic
port     = all
banaction = iptables-allports
logpath  = /var/log/messages
maxretry = 2
bantime  = 20m
```

`/var/log/messages` si se modifica en las definiciones de PAM, también se debe cambiar aquí

### desbloquear IP solo en el servicio ssh

`fail2ban-client establece sshd unbanip 10.10.1.2`

### desbloquear todas las ips

```
fail2ban-cliente unban --all

/sbin/service fail2ban restart

/sbin/service syslog restart
```

### verificar reglas administradas en el firewall

```
fail2ban-client -vd

iptables -L
```

## Cuestiones y notas importantes

El entorno de la red pública puede ser complejo y peligroso, con innumerables
Los dispositivos maliciosos escanean constantemente direcciones IP en Internet. Esto significa
que el uso de Fail2ban en una red pública requiere un monitoreo constante de los registros y
actualizaciones frecuentes de las reglas del firewall.

### dirección estática versus dirección dinámica

Utilice esta herramienta con cuidado y agregue cualquier dirección IP que su servicio necesite
conectarse como "ignorado" Las IP, por supuesto, deben ser una dirección estática.

### Bloqueo accidental e interrupción del servicio

Si está utilizando Fail2ban en un entorno de red complejo o en un dispositivo de gama baja,
Es posible que deba prestar especial atención al monitorear el estado del servicio.

Si tiene problemas de red, con una conexión inestable, cualquier servicio TCP que
estar orientado a la conexión de red sufrirá prohibiciones, porque los intentos constantes
de reconexiones en fallos podrían interpretarse como ataques DOS o sondeos falsos.

## Ver también

* [README.md](README.md)

## LICENCIA

**CC BY-NC-SA**: el proyecto permite a los reutilizadores distribuir, remezclar, adaptar y construir sobre el material.
en cualquier medio o formato únicamente con fines no comerciales, y únicamente siempre que se proporcione la atribución
a los creadores involucrados. Si remezcla, adapta o construye sobre el material, debe obtener la licencia del material modificado.
material bajo idénticos términos, incluye los siguientes elementos:

* **POR**: el crédito se debe otorgar al creador de cada contenido respectivamente, comenzando por el primer colaborador.
* **NC** – ¡Solo se permiten usos no comerciales de la obra, con excepciones si completa un problema aquí!
* **SA** – Las adaptaciones deben compartirse bajo los mismos términos, debes obedecer estos términos y no cambiarlos.

Para obtener más información, consulte [alpine/copyright.md](../../alpine/copyright.md)
