

## habilitar red

```
cat /etc/network/interfaces 
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp

auto eth0
iface eth0 inet dhcp
```

## Configurar wifi

Alpine como en la prehistoria, una basrua para la wifi, aun 
no se logra funciona con SSID especiales por ejemplo sin claves con ssid escondidas.

```
echo 1 > /proc/sys/net/ipv6/conf/all/disable_ipv6
apk wireless-tools wpa_supplicant wireless-tools-doc wpa_supplicant-doc
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

