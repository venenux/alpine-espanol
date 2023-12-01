# servidor alpino apc servicio de detección de UPS

`apcupsd` es un pequeño programa que se comunica con los dispositivos UPS, específicamente los dispositivos APC.

Para obtener información más avanzada, consulte el tutorial múltiples UPS maestro-esclavo que monitorean y controlan la red de pérdida de energía: [alpine-recetas-acpupsd-service-multimonitor.md](alpine-recetas-acpupsd-service-multimonitor.md)

## Servidor autónomo de monitoreo único UPS de APC

Este tutorial configurará el servidor conectado al UPS APC, por lo que
autocontrolará su estado de disponibilidad de energía.

Este tutorial permitirá que la máquina se apague automáticamente después de un corte de energía.

### preparación

* configurar un nombre de host, aquí usamos el hostname "develupscheck" como nombre de el servidor o manquina
* repositorios normales agregados y actualizados
* actualizar repositorios


```
hostname develupscheck

echo 'hostname="develupscheck"' > /etc/conf.d/hostname 

echo "develupscheck" > /etc/hostname

cat > /etc/apk/repositories << EOF; $(echo)
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```

### Instalación del servicio appupsd

* instalar paquetes de necesidades básicas y módulos appupsd
* agregar el servicio al inicio

```
apk add apcupsd eudev sed

rc-update add appupsd
```

> **Warning**: el paquete `eudev` es muy necesario para detectar automáticamente el dispositivo, el paquete `sed` es necesario porque la expresión regular extendida


Después de la instalación:

* Archivos de configuración
     * `/etc/apcupsd.conf` el archivo de configuración PRINCIPAL
     * `/etc/hosts.conf` otras computadoras soportadas por el mismo UPS (controlador de red NIS para esclavos)
     * `/etc/multimon.conf` Parámetros para mostrar en la interfaz web
* archivos de eventos
     * `/etc/apccontrol` aquí encontrará los distintos eventos
     * `/etc/changeme` envía correo electrónico para cambiar la batería del UPS
     * `/etc/commfailure` envía un correo electrónico cuando se pierde la conexión con el UPS
     * `/etc/commok` envía un correo electrónico cuando se establece la conexión con el UPS
     * `/etc/offbattery` envía un correo electrónico cuando su computadora funciona con energía
     * `/etc/onbattery` envía correo electrónico cuando su computadora funciona con batería (UPS)
* archivos de programas
     * `/sbin/apcaccess` muestra la informacion de(los) UPS configurados al invocarse
     * `/sbin/apctest` revisa las configuraciones y programas se ejecuten bien
     * `/sbin/apcupsd` es el servicio y programa como tal que esta constantemente ejecutando
* archivos de servicio
     * /etc/init.d/apcupsd` servicio demonio común para el software UPS de APC
     * /etc/init.d/apcupsd.powerfail` Igual que Debian crea el enlace simbólico `/etc/init.d/ups-monitor` → /etc/apcupsd/ups-monitor. Entonces, el script de detención de Debian (/etc/init.d/halt) lo ejecuta. De esta manera, corta la energía en el UPS con `apccontrol -killpower` después del apagado del sistema (si existe el archivo /etc/apcupsd/powerfail, que luego se elimina)
* archivos temporales
     * `powerfail` “Archivo de bandera” creado por apcupsd antes de apagar el sistema, para informar al script de parada que el apagado se debe a un corte de energía (fallo de energía)


### Configuracion - funcionamiento basico

El appupsd tiene valores predeterminados internos, los valores predeterminados son para tipos de dispositivos USB puros,
es decir, solo cable USB, hay algunos modelos que utilizan una SERIAL interna convertido a USB mediante un conector RJ45.
Se puede gestionar también por USB o red, pero debes encargarte de dicha configuración.

La parte más importante es la combinación DEVICE/UPSTYPE, los más problemáticos son los SmartUPS
con conectores SERIAL o RJ45, especialmente este último necesitará un cable especial llamado
como "cable de señalización inteligente" que puede ser SERIAL: http://www.apcupsd.org/manual/manual.html#smart-custom-cable-for-smartupses
o puede ser USBSERIAL: http://www.apcupsd.org/manual/manual.html#custom-rj45-smart-signalling-cable-for-backups-cs-models

Aquí los comandos para configurar el dispositivo usando el modelo ES 600M1 que es un dispositivo USB puro:

* configure UPSCABLE para el dispositivo detectado, **Advertencia** tenga cuidado con el anuncio del párrafo anterior
* establezca UPSTYPE en el dispositivo configurado, **Advertencia** tenga cuidado con el anuncio del párrafo anterior
* establezca POLLTIME en una cantidad mínima, por lo que comprobaremos con más frecuencia
* establezca ONBATTERYDELAY en un máximo de 12 para que los falsos positivos no apaguen la máquina
* configure BATTERYLEVEL al 40 por ciento para que apaguemos la máquina y no corramos riesgos con una fuente de energía vacía
* establecer MINUTOS en 10 por el mismo motivo del tema anterior
* establezca MOLESTAR en 100 segundos, no nos importan los usuarios registrados actualmente, el software y los datos que contiene son muy importantes
* establezca ANNOYDELAY en 20 segundos para que avisemos a los usuarios registrados sobre un apagado rápido
* habilitar el servicio después de configurarlo
* iniciar (o reiniciar) el servicio# servidor alpino apc servicio de detección de UPS

```
sed -Ei "s|^[[:space:]]?UPSCABLE.*|UPSCABLE usb|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?UPSTYPE.*|UPSTYPE usb|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?POLLTIME.*|POLLTIME 40|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?ONBATTERYDELAY.*|ONBATTERYDELAY 12|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?BATTERYLEVEL.*|BATTERYLEVEL 40|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?MINUTES.*|MINUTES 10|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?ANNOY.*|ANNOY 100|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?ANNOYDELAY.*|ANNOYDELAY 20|g" /etc/apcupsd/apcupsd.conf

rc-update add apcupsd

rc-service apcupsd restart
```

Después de configurar y reiniciar el servicio, puede probarlo mediante la línea de comando `apcaccess`:

```
$ apcaccess
APC      : 001,036,0856
DATE     : 2023-11-30 16:56:57 -0400  
HOSTNAME : develupscheck
VERSION  : 3.14.14 (31 May 2016) debian
UPSNAME  : develupscheck
CABLE    : USB Cable
DRIVER   : USB UPS Driver
UPSMODE  : Stand Alone
STARTTIME: 2023-11-30 16:56:55 -0400  
MODEL    : Back-UPS ES 600M1 
STATUS   : ONLINE 
LINEV    : 119.0 Volts
LOADPCT  : 7.0 Percent
BCHARGE  : 100.0 Percent
TIMELEFT : 93.3 Minutes
MBATTCHG : 5 Percent
MINTIMEL : 3 Minutes
MAXTIME  : 0 Seconds
SENSE    : Medium
LOTRANS  : 92.0 Volts
HITRANS  : 139.0 Volts
ALARMDEL : 30 Seconds
BATTV    : 13.5 Volts
LASTXFER : Low line voltage
NUMXFERS : 0
TONBATT  : 0 Seconds
CUMONBATT: 0 Seconds
XOFFBATT : N/A
SELFTEST : NO
STATFLAG : 0x05000008
SERIALNO : 4B2220P00773  
BATTDATE : 2022-05-17
NOMINV   : 120 Volts
NOMBATTV : 12.0 Volts
NOMPOWER : 330 Watts
FIRMWARE : 928.a9 .D U
END APC  : 2023-11-30 16:56:57 -0400
```


#### Instalación de red apcupsd-webif y servicio de monitor web

* instalar paquetes de necesidades básicas y módulos web apcupsd

```
apk add apcupsd apcupsd-webif sed lighttpd
```

> **Advertencia**: se debe configurar el servicio appupsd, incluso si el UPS no está en la máquina conectada

Debido a un error en el empaquetado debido a desarrolladores perezosos, `apcupsd` debe configurarse previamente https://t.me/alpine_linux_english/71210

### Configuración de apcupsd-webif - monitoreo de servicios web


Aquí la configuración para permitir la verificación desde cualquier host y habilitar el monitoreo de la red.

* Habilitar el parámetro NETSERVER
* Permitir que cualquier host revise este UPS, para filtrar debe usar la ip de la tarjeta de red, no admite segmentos de red
* El puerto debe ser el mismo del programa cgi si desea utilizar el monitor web cgi, ¡no lo cambie de 3551!
* habilitar alias, cgi, accesslog, módulo de lista de directorios en lighttpd
* crear el directorio cgi, el directorio localhost www y el directorio de caché
* montar el programa cgi compartido
* establecer permisos de propiedad
* configurar el servicio automáticamente
* directorio de montaje para la seguridad cgi usando alias (no copiar, sea profesional)
* reiniciar servicios


```
sed -Ei "s|^[[:space:]]?NETSERVER.*|NETSERVER on|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?NISIP.*|NISIP 0.0.0.0|g" /etc/apcupsd/apcupsd.conf

sed -Ei "s|^[[:space:]]?NISPORT.*|NISPORT 3551|g" /etc/apcupsd/apcupsd.conf

sed -i -r 's#\#.*mod_alias.*,.*#    "mod_alias",#g' /etc/lighttpd/lighttpd.conf
sed -i -r 's#.*include "mod_cgi.conf".*#   include "mod_cgi.conf"#g' /etc/lighttpd/lighttpd.conf
sed -i -r 's#\#.*mod_accesslog.*,.*#    "mod_accesslog",#g' /etc/lighttpd/lighttpd.conf
sed -i -r 's#\#.*dir-listing.activate*=.*,.*#dir-listing.activate   = "enable"#g' /etc/lighttpd/lighttpd.conf

mkdir -p /var/www/localhost/cgi-bin/apcupsd && mkdir -p /var/www/localhost/htdocs && mkdir -p /var/lib/lighttpd

chown -R lighttpd:lighttpd /var/www/localhost/ && chown -R lighttpd:lighttpd /var/lib/lighttpd && chown -R lighttpd:lighttpd /var/log/lighttpd

mount /usr/share/webapps/apcupsd /var/www/localhost/cgi-bin/apcupsd --bind

rc-update add lighttpd default

rc-service lighttpd restart
```

Después de eso, puede visitar el URI http://localhost/cgi-bin/acpupsd/multimon.cgi para verificar el servicio de monitorización web.


#### Configuraciones: avanzadas con eventos y scripts

Para configurar un evento personalizado, debe ver `apccontrol`, que apcupsd puede administrar (corte de energía, etc.);
Si desea realizar cambios, NO modifique este archivo directamente, sino cree un script con
el nombre de un evento y colóquelo en `/etc/apcupsd` (por ejemplo, un script personalizado `/etc/apcupsd/doshutdown`
se ejecutará primero cuando el evento `doshutdown` esté habilitado dentro de la definición `apccontrol`).


## ver también

* Apline master-slave multi ups monitorea y controla la red de pérdida de energía: [alpine-howto-acpupsd-service-multimonitor.md](alpine-howto-acpupsd-service-multimonitor.md)
* Recalibración de UPS para baterías agotadas http://www.apcupsd.org/manual/manual.html#recalibrating-the-ups-runtime

# LICENCIA

**CC BY-NC-SA**: el proyecto permite a los reutilizadores distribuir, remezclar, adaptar y construir sobre el material.
en cualquier medio o formato únicamente con fines no comerciales, y únicamente siempre que se proporcione la atribución
a los creadores involucrados. Si remezcla, adapta o construye sobre el material, debe obtener la licencia del material modificado.
material bajo idénticos términos, incluye los siguientes elementos:

* **POR**: el crédito se debe otorgar al creador de cada contenido respectivamente, comenzando por el primer colaborador.
* **NC** – ¡Solo se permiten usos no comerciales de la obra, con excepciones si completa un problema aquí!
* **SA** – Las adaptaciones deben compartirse bajo los mismos términos, debes obedecer estos términos y no cambiarlos.

