
```
apk add build-base linux-vanilla-dev linux-virt-dev
cd /usr/src/<moduloname>/
make menuconfig
make modules_prepare
make modules
make modules_install
modprobe <moduloname>
insmod ./<ruta>/<moduloname>.ko
```