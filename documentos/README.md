Alpine linux es originalmente una distro para dispositivos de redes.. 
su simplicidad y rapidez (por ser simple) **ha gustado a sus usuarios y 
estos empezaron meterle paquetes de todo tipo.. volviendo equivocamente en una todo uso**.

Esto le dara a ud **la razon de porque no ve muchos paquetes en main que otros tienen**, 
es porque esta originalmente enfocado a servicios orientados a redes.

Una **pista de esto es ver que paquetes como `kamailio`, `asterisk`, `php` 
esta muy al dia inclusive en versiones viejas como `3.6` y `3.7`.** Por ejemplo 
`lua` esta tanto la version 5.1, como 5.2 y 5.3, y php esta 5.6 y 7.1.

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

## documentos avanzados para machos

Todo lo anterior es para jevitas, aqui esta la cosa buena:

* Gestor de proyectos Redmine en [redmine-en-alpine.md](redmine-en-alpine.md), un entorno completisimo, 
similar a gitlab y github pero que soporta SVN, GIT, etc ademas su sistema wiki es mas avanzado usando macros.

* "Poderosisimo avanzado HTTP entry" en [alpine-http-avanzado.md](alpine-http-avanzado.md) 
a diferencia montara `lighttpd` + `mariadb` + `php` y si necesita `apache`, aprendera del poderosisimo "proxy-reverse".

* "Kamailio y Asterisk real time integration" en [alpine-voip-potenciado.md](alpine-voip-potenciado.md) 
para aquellos que desean montar `kamailio` + `asterisk` en modo tiempo real para clusterizado.

* "Configurar el poderoso COURIER mail service" en [alpine-courier-mailservice.md](alpine-courier-mailservice.md) 
porque esos estupidos posfix+dovecot+mysql es para las maricas.


# Vease tambien

* Indice general [../README.md](../README.md)
* Instalaciones: [../instalar/README.md](../instalar/README.md)
