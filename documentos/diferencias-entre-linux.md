

## Diferencias entre comandos de administracion


#### Difrencias direccion de red y activacion ###

| que comando | ifconfig (solo en ruta de root) | ip (puede correrse como usuario) |
| ----------- | ------------------------------- | -------------------------------- |
| ubicacion   | `/sbin/ifconfig`                | `/bin/ip`                        |
| ver ip (v4) | `ifconfig|grep "inet add"`      | `ip -4 addr|grep inet`           |
| ver activas | `ifconfig|grep "inet add"`      | `ip link ls up|grep mtu`         |
| bajar red   | `ifconfig enp0s0 down`          | `ip link set dev eth0 down`      |
| activar wifi | `ifconfig wl0s0 up`            | `ip link set dev wlan0 up`       |
| mas MTU red | `ifconfig enp1s0 mtu 9000 up`   | `ip link set mtu 9800 dev eth0`  |


#### Diferencias entre comandos de gestion de arranque y servicios ####

| comandos     | reboot               | shutdown                  | poweroff                  | halt                  | init                     |
| ------------ | -------------------- | ------------------------- | ------------------------- | --------------------- | ------------------------ |
| Debian       | ln -s /bin/systemctl | ln -s /bin/systemctl      | ln -s /bin/systemctl      | ln -s /bin/systemctl  | ln -s /lib/systemd/systemd |
| Mint         | bin                  | bin                       | ln -s reboot              | ln -s reboot          | bin                      |
| Void         | ln -s /usr/bin/halt  | posix sh script           | ln -s /usr/bin/halt       | bin                   | ln -s  /usr/bin/runit-init |
| Gentoo       | ln -s /usr/bin/halt  | bin                       | ln -s /usr/bin/halt       | bin                   | bin                      |
| Guix         | guile script         | ln -s halt                | not implemented           | guile script          | (from initrd)            |
| MacOS X      | bin                  | bin                       | not implemented           | bin                   | bin (launchd)            |
| NetBSD       | bin                  | ln reboot                 | ln reboot                 | ln reboot             | scrip/bin                |
| DragonflyBSD | bin                  | bin                       | ln shutdown               | ln reboot             | scrip/bin                |
| FreeBSD      | bin                  | bin                       | ln shutdown               | ln reboot             | scrip/bin                |
| OpenBSD      | bin                  | bin                       | not implemented           | ln reboot             | scrip/bin                |
| Fedora       | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/lib/systemd/systemd |
| CentOS       | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/lib/systemd/systemd |
| Arch Linux   | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/lib/systemd/systemd |
| OpenSuse     | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/bin/systemctl | ln -s /usr/lib/systemd/systemd |


# Vease tambien

* [README.md (indice documentos administrativos)](README.md)
* Instalaciones: [../instalar/README.md](../instalar/README.md)
* Guia de comandos para escritorio openbox y devel: [programas-esenciales-todo-en-uno.md](programas-esenciales-todo-en-uno.md)

