
Alpine es como las distros de antaño, y alli es su ventaja, la hace veloz. 
Esto significa que nada viene automatico, aqui estan los **comandos para 
despues de instalar a alpine funcionar como maquina de uso general** en su casa:

**ADVERTENCIA** Estas solo son notas y detalles, para receta completa usar el directorio [../recetas/](../recetas/)


## 1. habilitar red cableada

```
cat > /etc/network/interfaces << EOF
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
EOF

apk add dhcpcd
rc-update add dhcpcd default
rc-service dhcpcd start

ip link set eth0 up
```

**ADVERTENCIA** si hay mas de una interfaz sera `eth0`, `eth1`

## 2. Configurar ssh

```
sed "s|#UseDNS.*|UseDNS no|g" -i /etc/ssh/sshd_config
sed "s|#ListenAddress 0.*|ListenAddress 0.0.0.0|g" -i /etc/ssh/sshd_config
rc-update add sshd
```

**NOTA** por defecto root solo entra usando llaves o certificados pem, para permitir clave:

```
sed "s|^[#]*PermitRootLogin.*|PermitRootLogin yes|g" -i /etc/ssh/sshd_config
```

## 3. Configurar sonido:

```
apk add alsa-utils alsa-utils-doc alsa-lib alsaconf
modprobe snd-pcm-oss
modprobe snd-mixer-oss
sed -i '/snd-pcm-oss/d' /etc/modules
sed -i '/snd-mixer-oss/d' /etc/modules
echo -e "snd-pcm-oss\nsnd-mixer-oss" >> /etc/modules
rc-service alsa start
rc-update add alsa
```

## 4. Preparar un usuario para escritorio grafico:


* En `users` porque es un usuario normal, importante y en `games` para usar los juegos.
* En `audio` y `video` para poder ver archivos de sonido y peliculas, 
* En `plugdev`y `dialout` para hacer conexciones de red o internet asi como vpn o de modems.
* En `usb`, `disk`, `cdrw` y `cdrom` para poder usar los medios/discos removibles o grabar CD/DVD.

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


## 4. habilitar red grafica

```
apk add dhcpcd
rc-update add dhcpcd default
rc-service dhcpcd start
apk add wireless-tools wpa_supplicant
rc-update add wpa_supplicant default
rc-service wpa_supplicant start
apk add modemmanager usb-modeswitch usb-modeswitch-udev
apk add modemmanager networkmanager network-manager-applet
rc-update add networkmanager default
rc-service networkmanager start
```

# Avanzadas


## Configurar wifi consola

Alpine como en la prehistoria, una basrua para la wifi, aun 
no se logra funciona con SSID especiales por ejemplo sin claves con ssid escondidas.

```
echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6

apk add dhcpcd wireless-tools wpa_supplicant wireless-tools-doc wpa_supplicant-doc
rc-update add wpa_supplicant
rc-update add dhcpcd
rc-service start dhcpcd

ip link set wlan0 up
iwlist wlan0 scanning
iwconfig wlan0 essid nombredemired

cat > /etc/network/intefaces << EOF
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

auto wlan0
allow-hotplug wlan0
iface wlan0 inet dhcp
wireless-essid nombredemired
EOF

cat > /etc/wpa_supplicant/wpa_supplicant.conf << EOF
network={
ssid="nombredemired"
}
EOF

rc-service wpa_supplicant start
```

## Wifi sin clave o abierta

En la configuracion de "network" de `/etc/wpa_supplicant/wpa_supplicant.conf` 
remover toda referencia clave y colocar `key_mgmt=NONE`, un ejemplo:

```
network={
ssid="nombredemired"
key_mgmt=NONE
}
```

# Veae tambien:

1. Instalarla: [../instalar/README.md](../instalar/README.md)
2. Escritorio multimedia: [../recetas/programas-escritorio-xfce4-uso-general.md](../recetas/programas-escritorio-xfce4-uso-general.md)
3. Comandos utiles si esta perdido: [../recetas/comandos-iniciados.md](../recetas/comandos-iniciados.md)) 
