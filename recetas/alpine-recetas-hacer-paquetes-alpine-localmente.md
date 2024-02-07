
Aqui se aprendera crear y hacer un repositorio de `APKBUILD` (recetas) 
para hacer paquetes para [Alpine linux](../README.md)
tambien para configurar `abuild` para que puedas crear los paquetes 
a partir de los `APKBUILD`

### Como instalar abuild

Para hacer los paquetes necesitaremos abuild intalado
abriremos una terminal y ejecutaremos el comando ,`doas apk add abuild`, debe tener configurado `doas`

```bash
doas apk add abuild
```

**para hacerle la vida más fácil a la hora de empaquetar, es recomendable crear un nuevo usuario**

`adduser general`

luego de haber creado dicho usuario, debe darle permiso en `/etc/sudoers`
añada la línea usando el comando `visudo`:

`general ALL=(ALL) ALL` una línea por debajo de

User privilege specification

``root ALL=(ALL) ALL``

Ahora cierre la sesión de la cuenta de root, e inicie sesión como `general`. A partir de aquí todo se puede hacer en una cuenta de usuario normal, y las operaciones que requieren privilegios de superusuario se pueden hacer con sudo.

### Configurando git

Debe configurar git en su nueva sesion de usuario

`git config --global user.name "tu nombre como esta en gitlab"`

`git config --global user.email "tuusario@tucorreoelectronico.com"`

Antes de empezar a crear o modificar archivos APKBUILD, necesitamos darle permisos de `abuild` al usuario creado.
Edite el archivo abuild.conf según sus necesidades, desde la terminal:

`sudo addgroup general abuild`

También necesitamos preparar la ubicación donde el proceso de compilación almacena
en caché los archivos que se descargan, por defecto es `/var/cache/distfiles/`, para crear este directorio y asegurarse de 
que tiene permisos de escritura, introduzca los siguientes comandos:

`sudo mkdir -p /var/cache/distfiles`

`sudo chmod a+w /var/cache/distfiles`

`sudo chgrp abuild /var/cache/distfiles`

`sudo chmod g+w /var/cache/distfiles`

El último paso es configurar las claves de seguridad con el script `abuild-keygen` para abuild con el comando: 

`abuild-keygen -a -i`

En versiones anteriores de Alpine, teníamos que crear manualmente claves para firmar paquetes e índices. Esto explica cómo, hoy en día se puede usar `abuild-keygen`.
Dado que la clave pública debe ser única para cada desarrollador, la dirección de correo electrónico debe utilizarse como nombre de la clave pública. 

#### Creando una llave privada

`openssl genrsa -out tucorreoelectronico.priv 2048`

#### Creando una llave publica

`openssl rsa -in tucorreoelectronico.priv -pubout -out /etc/apk/keys/tucorreoelectronico`

La llave pública debe ser distribuida e instalada en /etc/apk/keys la caja de alpine 
que instalará los paquetes, esto significa básicamente que las llaves públicas del desarrollador principal 
deberían estar en /etc/apk/keys en todas las cajas Alpine.

### Para crear los paquetes con abuild

Entraremos en la carpeta donde estan los paquetes con el comando `cd`,
dentro de la carpeta de los paquetes usaremos. 

`cd nombre del paquete`.

Ya adentro de la carpeta del nombre del paquete ejecutaremos el comando `abuild`.

# mas informacion

- Author
  - Sodomon as https://t.me/alpine_linux_espanol/26413
- 🗯 IRC
  - 💬 `##alpine_telegram_english` (roto)
- 📱 Telegram https://t.me/alpine_linux
  - 🇨🇴 https://t.me/alpine_linux_espanol
  - 📡 https://t.me/latam_programadores
- Matrix
  - 👥 https://matrix.to/#/#alpine-linux-espanol:matrix.org

# LICENSE

**CC BY-NC-SA** Sodomon 2020
