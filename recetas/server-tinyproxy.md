# servidor alpine Proxy browse network

`proxy` termino que se aplica al servico en el medio de la comunicacion.

Hay varios tipos de proxy, en este caso usaremos uno de pasarela para navegacion 
en internet, esto es que a travez de el servidor proxy navegaran otros 
dispositivos o maquinas.

## Que utilidad tiene?

* **Navegar como si fuera otra maquina** la maquina que navega se conecta al 
proxy, y la maquina proxy es la que dara la cara, significa que para efectos 
del servidor a visitar o la pagina web visitada, la maquina que esta visitando 
no es sino la del proxy, y la maquina que se conecta al proxy permanece 
parcialmente oculta.
* **Navegacion para dispositivos confinados** si tienes muchos dispositivos o 
maquinas que estan bloqueadas o estan detras de un firewall, puedes configurar 
el proxy para que todas estas navegen a travez del mismo, obviamnete el server 
que tiene el proxy debe ser una maquina accesible tanto por estas maquinas 
limitadas asi como que pueda navegar para que el proxy otorge la funcionalidad.
* **NAvegacion para dispositivos limitados** este ejemplo es el mas usado, 
si la señal de internet es wifi pero no tienes mas equipos wifi, puedes hacer 
que la maquina con wifi le de internet a las que no tienen usando el servicio 
del proxy, asi las que no tienen wifi si todas estan tambien a la red, la que 
tiene wifi se configura como proxy en las que no tiene wifi, obviamente esto 
significa que la maquina con wifi tendria dos interfaces de red al menos, una 
para obtener el internet y la otra interfaz cableada para dar el proxy.

## Que utilidad tenia?

Antiguamente se usaba parau simular una internet mas rapida, esto era porque 
el servidor proxy guardaba las paginas y recursos que ya se habian visitado, 
por lo que cada vez que una maquina le pedia navegar al proxy navegaba es la 
pagina guardada lo que simulaba una velocidad mayor.

Hoy dia esto se le llama entonces "proxy cache" y solo se usa como medio de 
limitacion o como medio para rendimiento, pero no es muy optimo dado el 
internet hoy dia es tremendamente rapido aun en velocidades lentas.

## Instalacion de tiniproxy

Este tutorial configurará el servidor como un servidor proxy pero sin cache.

### preparación

* configurar un nombre de host para que no rechaze nuestros paquetes
* repositorios normales agregados y actualizados
* actualizar repositorios


```
hostname venenux && echo 'hostname="venenux"' > /etc/conf.d/hostname && echo "venenux" > /etc/hostname

cat > /etc/apk/repositories << EOF; $(echo)
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```

### Instalación del servicio tinyproxy

* instalar paquetes minimos
* agregar el servicio al inicio

```
apk add tinyproxy tinyproxy-doc tinyproxy-openrc

rc-update add tinyproxy
```

> **Warning**: el paquete `eudev` es muy necesario para detectar automáticamente el dispositivo, el paquete `sed` es necesario porque la expresión regular extendida


Después de la instalación:

* Archivos de configuración
     * `/etc/tinyproxy/tinyproxy.conf` el archivo de configuración PRINCIPAL
     * `/etc/tinyproxy/filter` archivo de lista blanca/negra sea por URL o dominio
* archivos de programas
     * `/usr/bin/tinyproxy` el ejecutable que a su vez tambien e el demonio
* archivos de servicio
     * /etc/init.d/tinyproxy` script openrc por alpine
     * /var/log/tinyproxy/tinyproxy.log` archivo de log asumido por el daemon, debe ser igual en el conf


### Configuracion - funcionamiento basico

El paquete de alpine no biene nada bien configurado, hay que ajustarlo para que 
funcione con el servicio.

Esto es porque en los dockers y containers se necesita que `tinyproxy` funcione 
sin ser un servicio que pertenezca a nadie, por eso no esta atado al grupo ni 
tampoco al usuario en el archivo de configuracion.

La parte más importante es la combinación de los filtros, esto se abordara en 
el apartado de funcionamiento avanzado. Aqui se configurara para un servicio 
en una pc:

* configurar para ejecutar en su propio usuario y grupo el servicio
* configurar el puerto, los proxy web usan 8888, use este para compatibilidad
* si tiene una sola tarjeta de red, usar la misma para la entrada y salida
* configurar el maximo de maquinas que pueden usar este proxy a 10
* permitir que cualquier ip se conecte y use el proxy
* configurar el log y run igual que el servicio de arranque demonio
* habilitar el servicio y iniciar (o reiniciar) el servicio de proxy

```
sed -Ei 's|^.?User.*|User tinyproxy|g' /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?Group.*|Group tinyproxy|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?Port.*|Port 8888|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?BindSame.*|BindSame yes|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?MaxClients.*|MaxClients 10|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?Allow[[:space:]]|#Allow |g' /etc/tinyproxy/tinyproxy.conf

mkdir -p /var/run/tinyproxy && chown tinyproxy /var/run/tinyproxy
sed -Ei 's|^.?LogFile.*|LogFile "/var/log/tinyproxy/tinyproxy.log"|g' /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?PidFile.*|PidFile "/var/run/tinyproxy.pid"|g' /etc/tinyproxy/tinyproxy.conf
rc-update add tinyproxy && rc-service tinyproxy restart
```

Después de configurar y reiniciar el servicio, puede probarlo mediante 
esta línea de comando en shell asi:

```
apk add curl curl-doc

export http_proxy="http://localhost:8888/"} && curl -I http://45.181.69.141
```

### Configuracion avanzadas

#### Configurar tinyproxy para navegacion oculta

```
sed -Ei 's|^.?User.*|User tinyproxy|g' /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?Group.*|Group tinyproxy|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?Port.*|Port 8888|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?BindSame.*|BindSame yes|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?Allow[[:space:]]|#Allow |g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?DisableViaHeader.*|DisableViaHeader Yes|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?XTinyproxy.*|XTinyproxy No|g' /etc/tinyproxy/tinyproxy.conf

viaproxyname=$(head -n1 < <(fold -w10 < <(tr -cd 'a-z0-9' < /dev/urandom)))
sed -Ei "s|^.?ViaProxyName.*|ViaProxyName \"${viaproxyname}\"|g" /etc/tinyproxy/tinyproxy.conf

mkdir -p /var/run/tinyproxy && chown tinyproxy /var/run/tinyproxy
sed -Ei 's|^.?LogFile.*|LogFile "/var/log/tinyproxy/tinyproxy.log"|g' /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?PidFile.*|PidFile "/var/run/tinyproxy/tinyproxy.pid"|g' /etc/tinyproxy/tinyproxy.conf
rc-update add tinyproxy && rc-service tinyproxy restart
```

#### Configurar tinyproxy para filtrar que se navega

```
touch /etc/tinyproxy/filter && echo "mocosoft" >> /etc/tinyproxy/filter
touch /etc/tinyproxy/filter && echo "*.facebook.com" >> /etc/tinyproxy/filter

sed -Ei 's|^.?Filter[[:space:]].*|Filter \"/etc/tinyproxy/filter\"|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?FilterURLs[[:space:]].*|FilterURLs On|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?Allow[[:space:]]|#Allow |g' /etc/tinyproxy/tinyproxy.conf

mkdir -p /var/run/tinyproxy && chown tinyproxy /var/run/tinyproxy
sed -Ei 's|^.?LogFile.*|LogFile "/var/log/tinyproxy/tinyproxy.log"|g' /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?PidFile.*|PidFile "/var/run/tinyproxy.pid"|g' /etc/tinyproxy/tinyproxy.conf
rc-update add tinyproxy && rc-service tinyproxy restart
```

#### Configurar tinyproxy para filtrar quien puede navegar

```
default_if=$(ip route list | awk '/^default/ {print $5}')
netmaskip=$(ip -o -f inet addr show ${default_if} | awk '{print $4}')

sed -Ei "s|^.?Allow[[:space:]]127.0.0.1|#Allow ${netmaskip}|g" /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?Allow[[:space:]]127.0.0.1|#Allow 127.0.0.1|g' /etc/tinyproxy/tinyproxy.conf

sed -Ei 's|^.?MaxClients.*|MaxClients 10|g' /etc/tinyproxy/tinyproxy.conf

mkdir -p /var/run/tinyproxy && chown tinyproxy /var/run/tinyproxy
sed -Ei 's|^.?LogFile.*|LogFile "/var/log/tinyproxy/tinyproxy.log"|g' /etc/tinyproxy/tinyproxy.conf
sed -Ei 's|^.?PidFile.*|PidFile "/var/run/tinyproxy.pid"|g' /etc/tinyproxy/tinyproxy.conf
rc-update add tinyproxy && rc-service tinyproxy restart
```

# LICENCIA

**CC BY-NC-SA**: el proyecto permite a los reutilizadores distribuir, remezclar, adaptar y construir sobre el material.
en cualquier medio o formato únicamente con fines no comerciales, y únicamente siempre que se proporcione la atribución
a los creadores involucrados. Si remezcla, adapta o construye sobre el material, debe obtener la licencia del material modificado.
material bajo idénticos términos, incluye los siguientes elementos:

* **POR**: el crédito se debe otorgar al creador de cada contenido respectivamente, comenzando por el primer colaborador.
* **NC** – ¡Solo se permiten usos no comerciales de la obra, con excepciones si completa un problema aquí!
* **SA** – Las adaptaciones deben compartirse bajo los mismos términos, debes obedecer estos términos y no cambiarlos.

