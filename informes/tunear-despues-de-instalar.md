

## habilitar red consola

```
apk add wireless-tools wpa_supplicant wireless-tools-doc wpa_supplicant-doc
rc-update add wpa_supplicant default
rc-service wpa_supplicant start
apk add networkmanager
rc-update add networkmanager
rc-service networkmanager start

cat > /etc/network/interfaces << EOF
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp

auto eth0
iface eth0 inet dhcp
EOF
```

## habilitar red grafica

```
apk add wireless-tools wpa_supplicant wireless-tools-doc wpa_supplicant-doc
rc-update add wpa_supplicant default
rc-service wpa_supplicant start
apk add modemmanager networkmanager network-manager-applet network-manager-applet-doc
rc-update add networkmanager default
rc-service networkmanager start
```

Cada usuario debe estar en el grupo `plugdev` para poder alterar coneciones de sistema.



## Configurar wifi consola

Alpine como en la prehistoria, una basrua para la wifi, aun 
no se logra funciona con SSID especiales por ejemplo sin claves con ssid escondidas.

```
echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6
apk add wireless-tools wpa_supplicant wireless-tools-doc wpa_supplicant-doc
ip link set wlan0 up
iwlist wlan0 scanning
iwconfig wlan0 essid venenuxwifi
iwconfig wlan0
```

# Configurar ssh

```
sed "s|#UseDNS.*|UseDNS no|g" -i /etc/ssh/sshd_config
sed "s|#ListenAddress 0.*|ListenAddress 0.0.0.0|g" -i /etc/ssh/sshd_config
rc-update add sshd
```


# Preparar un usaurio para escritorio grafico:

En `users` porque es un usuario normal, importante y en `games` para usar los juegos.
En `audio` y `video` para poder ver archivos de sonido y peliculas, 
En `plugdev`y `dialout` para hacer conexciones de red o internet asi como vpn o de modems.
En `usb`, `disk`, `cdrw` y `cdrom` para poder usar los medios/discos removibles o grabar CD/DVD.


```
adduser general
Changing password for general
New password: 
Retype password: 
passwd: password for general changed by root

adduser general audio
adduser general video
adduser general plugdev
adduser general users
adduser general games
adduser general usb
adduser general cdrw
adduser general dialout
adduser general disk
adduser general cdrom

```
