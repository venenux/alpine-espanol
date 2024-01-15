Alpine linux es originalmente una distro para dispositivos de redes salida de LEAF linux.. 
su simplicidad y rapidez (por ser simple) **ha gustado a sus usuarios y 
estos empezaron meterle paquetes de todo tipo.. creyendola equivocamente en una todo uso**.

Esto le dara a ud **la razon de porque no ve muchos paquetes en main que otros tienen**, 
es porque esta originalmente enfocado a servicios orientados a redes. Los paquetes 
que no son de este enfoque estan "todos apiñados" en un repositorio llamado "community".

Una **pista de esto es ver que paquetes como `kamailio`, `asterisk`, `php` 
esta muy al dia inclusive en versiones viejas como `3.6` y `3.7`.** Por ejemplo 
`lua` esta tanto la version 5.1, como 5.2 y 5.3, y php esta 5.6 y 7.1 desde hace mucho 
en los repositorios de alpine y si estan en comunidad mucho mas desde antes.

## Comenzando con linux

* Entorno de consola, que es una shell y como configurarla en alpine [alpine-arrancando-shells.md](alpine-arrancando-shells.md)

## Documentos desarrollo:

* Entorno de desarrollo separado [alpine-packaging-chroot.md](alpine-packaging-chroot.md), 
le permite usar alpine en cualqueir otro linux sin instalar paquetes de sobra o sin instalar en otra particion.

## documentos servicios estandar

En este indice encontrara:

* "Diferencias para comandos y administracion" en [diferencias-entre-linux.md](diferencias-entre-linux.md) 
ya que alpine por el busybox, sino se le indica difiere en el uso de los comandos y aqui se da conocer porque.

* "Como montar un LAMP stock" en [alpine-lamp-recetario.md](alpine-lamp-recetario.md) 
lo que llaman "linux + `apache` + `mysql` + `php`" esta aqui todo explicado.

* "Introduccion a voip con alpine" en [alpine-voip-introduccion.md](alpine-voip-introduccion.md) 
donde se enseñara configurar y montar un sistema de comunicacion de voz tipo central telefonica.

* "Montar un servicio de correo sencillo" en [alpine-mail-recetario.md](alpine-mail-recetario.md) 
empleando los paquetes conocidos desde el repositorio sin complicarse con filtrados ni antivirus estupidos.

* Gestor de proyectos Redmine en [redmine-en-alpine.md](redmine-en-alpine.md), un entorno completisimo, 
similar a gitlab y github pero que soporta SVN, GIT, etc ademas su sistema wiki es mas avanzado usando macros.

* Gestionar proteccion con fail2ban en [alpine-fail2ban-avanzado.md](alpine-fail2ban-avanzado.md), 
incluyendo proteccion euristica y variable, opciones y explicacion de como trabaja.

## documentos avanzados para machos

Todo lo anterior es para jevitas, aqui esta la cosa buena:

* "Poderosisimo avanzado HTTP entry" en [alpine-http-avanzado.md](alpine-http-avanzado.md) 
a diferencia montara `lighttpd` + `mariadb` + `php` y si necesita `apache`, aprendera del poderosisimo "proxy-reverse".

* "Kamailio y Asterisk real time integration" en [alpine-voip-potenciado.md](alpine-voip-potenciado.md) 
para aquellos que desean montar `kamailio` + `asterisk` en modo tiempo real para clusterizado.

* "Configurar el poderoso COURIER mail service" en [alpine-courier-mailservice.md](alpine-courier-mailservice.md) 
porque esos estupidos posfix+dovecot+mysql es para las maricas.

# Vease tambien

* Indice general [../README.md](../README.md)
* Instalaciones: [../instalar/README.md](../instalar/README.md)
