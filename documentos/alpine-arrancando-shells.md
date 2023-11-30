## shell alpine y terminal

> **Nota**: Debes tener Alpine ya instalado [alpine-newbie-install.md](alpine-newbie-install.md)

Respuesta corta:

* **terminal** = entorno de entrada/salida de texto, su computadora es al mismo tiempo una terminal
* **consola** = terminal física, cualquier cosa que uses como terminal pero eso no significa una computadora completa
* **shell** = intérprete de línea de comando, es el software que se muestra dentro de una terminal de consola

En Alpine, la terminal predeterminada siempre le proporcionará "ash" a través de "busybox", bastante limitada pero bastante funcional.

## Shell de consola predeterminado

El sistema operativo no tiene una interfaz oficial, podría ser
uno de consola o una gráfica.

| Nombre     | Nombre del paquete del shell        | desde | programas principales |
| ---------- | ----------------------------------- | ----- | --------------- |
| bash       | bash bash-doc                       | 3.0   | `/bin/bash`, `/usr/lib/bash/` |
| dash       | dash dash-doc                       | 3.17  | `/bin/dash` |
| csh        | tcsh tcsh-doc                       | 3.4   | `/bin/csh`, `/bin/tcsh`, `/etc/tcsh.cshrc` |
| zsh        | zsh zsh-vcs zsh-zftp zsh-doc        | 3.4   | `/bin/zsh`, `/usr/lib/zsh/` |
| fish       | fish fish-tools fish-dev fish-doc   | 3.10  | `/usr/bin/fish`, `/etc/fish/config.fish`, `/usr/share/fish/` |
| nushell    | nushell nushell-plugins nushell-doc | edge  | `/usr/bin/nushell` |

#### cambiar manualmente el shell de la consola predeterminado

> **Nota**: no hay ningún shell instalado de forma predeterminada, sólo `ash`

* Instale el shell deseado
* Verifique que el shell deseado sea válido para el sistema
* definición del archivo seds para cambiar el shell predeterminado del usuario `general` a `bash`


```
apk add bash bash-doc sed

grep bash /etc/shells

sed -e '/general/ s#\:[^\:]*$#\:/bin/bash#g' /etc/passwd
```

> **Advertencia**: si el tercer paso no muestra algo, no puedes usar dicho shell (en este ejemplo, `bash`)

Esto se puede ejecutar con un solo comando.
como `apk add bash sed && sed -e '/general/ s#\:[^\:]*$#\:"$(grep bash /etc/shells)"#g' /etc/passwd`

#### Cambio administró el shell predeterminado con libuser

> **Nota**: `libuser` es solo desde Alpine v3.14

* Instale el shell deseado
* Instalar el programa shell del administrador
* Crear archivos necesarios
* Verifique que el shell deseado sea válido para el sistema
* Cambiar el shell predeterminado del usuario `general` a `bash`

```
apk add bash bash-doc grep

apk add libuser libuser-doc

toque /etc/login.defs
mkdir /etc/default/ && toque /etc/default/useradd

grep bash /etc/shells

general lchsh
```

> **Advertencia**: si el tercer paso no muestra algo, no puedes usar dicho shell (en este ejemplo, `bash`)

Este programa es interactivo, le pedirá la ruta completa al
shell ejecutable y se puede ejecutar con un solo comando
como `apk add bash grep libuser && lchsh general`

#### El cambio administró el shell predeterminado con sombra

> **Nota**: `shadow` no está instalado de forma predeterminada, solo Busybox

* Instale el shell deseado
* Instalar el programa shell del administrador
* Crear archivos necesarios
* Verifique que el shell deseado sea válido para el sistema
* Cambiar el shell predeterminado del usuario `general` a `bash`

```
apk add bash bash-doc grep

apk add sombra sombra-doc

touch /etc/login.defs
mkdir /etc/default/ && touch /etc/default/useradd

grep bash /etc/shells

chsh -s /bin/bash general
```

> **Advertencia**: si el tercer paso no muestra algo, no puedes usar dicho shell (en este ejemplo, `bash`)

Esto se puede ejecutar con un solo comando.
como `apk add bash grep sombra && chsh -s $(grep bash /etc/shells) general`

#### error de shel no valido o el programa shell no existe

Esto **fue un gran error en las versiones alpine: https://gitlab.alpinelinux.org/alpine/aports/-/issues/11164**

En cada una de las opciones de este documento se puso un paso que decía: "Verifique que el shell deseado sea válido para el sistema",
bueno, cada shell debe estar presente en el archivo `/etc/shells` antes de aplicar.

> **Nota**: Si el shell se compila manualmente y no está empaquetado, ¡debe agregarlo al archivo! `/etc/shells`

#### como agregar una version mas reciente o recien empaquetada

Esto depende, consulte varios ejemplos:

* si estás usando Alpine 3.8 y quieres un shell más reciente como `fish`, **¡puedes!**
* si estás usando Alpine 3.8 y quieres un shell más reciente como `nushell`: **no puedes porque está muy lejos de 3.8**
* si estás usando Alpine 3.12 y quieres un shell más reciente como `fish`: **¡puedes!**
* si está utilizando Alpine 3.17 o 3.18 y desea un shell más reciente como `nushell`: **¡puede hacerlo!**

Esto significa que usted:

* actualizar los repositorios de la versión actual
* instalar las dependencias requeridas (verifique el nombre del paquete en pkgs.alpinelinux.org
* agregar el repositorio de la siguiente versión inmediata
* instalar la nueva versión más actualizada
* comprobar los archivos requeridos
* cambiar el shell para el usuario deseado
* eliminar el repositorio adicional inmediato, configurando los normales nuevamente
* Pruebe si funciona; de lo contrario, deberá actualizar a Next Alpine o usar archivos estáticos apk para reparar World DB.

A modo de ejemplo, esto le muestra cómo instalar Nushell en Alpine 3.17 o 3.18 que está cerca del borde (como 2023):

```
cat > /etc/apk/repositories << EOF; $(echo)
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update && apk add shadow shadow-doc grep busybox-binsh libcrypto3 libgcc libssl3 sqlite-libs

cat > /etc/apk/repositories << EOF; $(echo)
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://dl-cdn.alpinelinux.org/alpine/edge/main
http://dl-cdn.alpinelinux.org/alpine/edge/community
EOF

apk update && apk add nushell nushell-plugins nushell-doc

touch /etc/login.defs && mkdir /etc/default/ && touch /etc/default/useradd

chsh -s $(grep nushell /etc/shells) general

cat > /etc/apk/repositories << EOF; $(echo)
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```

### Acerca del material protegido por derechos de autor

**CC BY-NC-SA**: Si remezclas, adaptas o construyes sobre el material, debes obtener la licencia del material modificado.
material bajo idénticos términos, incluye los siguientes elementos:

* **POR**: el crédito se debe otorgar al creador de cada contenido respectivamente, comenzando por el primer colaborador.
* **NC** – ¡Solo se permiten usos no comerciales de la obra, con excepciones si completa un problema aquí!
* **SA** – Las adaptaciones deben compartirse bajo los mismos términos, debes obedecer estos términos y no cambiarlos.

