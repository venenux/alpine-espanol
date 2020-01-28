
Either way, this section describes, step by step, how to get 
and install usage of main setup-* utilities, as well as explanations 
for a complete install on common computer BIOS or UEFI based, 
the Grub loader supports EFI base and GPT partitions indexing, 
a DELL Vostro 230 was used specifically for this document.

# 0. Source media

First download a source media, from https://alpinelinux.org/downloads/, 
all the images will need network internet connection except `extended` 
due comes with minimal need packages but are x86 based only.

For all users no cares from what operating system comes or wicht architecture, 
we recommended to use `balena-etcher-electron` of https://www.balena.io/etcher/ 
for flashing USB drive from any system, of course you must be run 
as root or admin user.

* download the media image iso file
* download the balena-etcher-electron program
* run the balena-etcher-electron program as root under X session
* click on the icon of "select image" and open the image iso file downloaded
* plug in on the computer the USB drive, it will automatically show it
* then only umount but not eject the device! be sure you backup all the files on the USB drive!
* after the program balena-etcher-electron show the SUB drive as "sdb" click on `flash`
* waith for a while and when finish, close and remove usb to plugin into install target computer

# 1. Installation

After flash the USB media or burn a CDROM media, put the media install 
on respective drive bay of the computer and turn on the computer.

Select proper boot media, on DELL Vostro always are the `F12` key, press 
at the boot "Dell" screen and when menu shows select the proper media, 
on the VirtualBox software are same `F12` key too, by hitting that key 
a boot selection media will be displayed, boot screen depends on each computer; 
in this case USB drive was used, 
rest of this document makes no sense which media was used.

## 1.1. Installation from scrat

Installation was made without problems using 3.11.3 except for some 
repositories does not resolve hostnames, but for 3.10.3 are the same, 
and then can upgrade to 3.11.3 as described in next optional section; 
so we used the 3.10.3 images for install:

```
setup-keymap us us-altgr-intl

cat > /root/autofile << EOF
KEYMAPOPTS="us us"
HOSTNAMEOPTS="-n alpine-test"
DNSOPTS="8.8.8.8"
TIMEZONEOPTS="-z UTC"
PROXYOPTS="none"
APKREPOSOPTS="-1"
SSHDOPTS="-c openssh"
NTPOPTS="-c chrony"
DISKOPTS="-m sys -s 4096 /dev/sda"
EOF

export BOOT_SIZE=500
export SWAP_SIZE=4096
export BOOTLOADER=grub

setup-alpine -f /root/autofile
```

Be care of the network questions and root password questions in the 
process due will be used later and must be mandatory.

This will make the disk layout as:

* `/dev/sda1` como BOOT en 500Mb en `/boot`
* `/dev/sda2` como SWAP en 4Gb
* `/dev/sda3` como ROOT en 200Gb en `/`

but this will be only if the autofile its used and make it property.

## 1.2  Installation from upgrade

With this method you already have a instalation from scrat, 
you can upgrade from previous version 3.X to lasted 3.11.3 version, 
only change your apk repositories with following commands, 
for later run the update and upgrade apk commands as:

```
sed -s -i -r 's|v$(cat /etc/alpine-release | cut -d'.' -f1,2)|v3.11|g' /etc/apk/repositories

apk update

apk upgrade -a
```

## 1.3 Post install setup

We need minimal important packages and setups for console and development, 
also some specific settings for remote connection from others linux, 
due today Alpine not have a good remote x11 software than x11vnc.. 

The most recommended it's having a access user here named "remote" 
and normal general usage user here named "general" for convenience, 
in the next commands we will setup a very hardened limited environment 
for any new user and created those two users:

```
mkdir -p /etc/skel/

cat > /etc/skel/.logout << EOF
history -c
/bin/rm -f /opt/remote/.mysql_history
/bin/rm -f /opt/remote/.history
/bin/rm -f /opt/remote/.bash_history
EOF

cat > /etc/skel/.cshrc << EOF
set autologout = 30
set prompt = "$ "
set history = 0
set ignoreeof
EOF

cp /etc/skel/.cshrc /etc/skel/.profile

adduser -D --home /opt/remote --shell /bin/ash remote

echo "secret_new_remote_user_password" | chpasswd

adduser -D --shell /bin/bash general

echo "secret_new_general_user_password" | chpasswd
```

Before this if you have wired connection internet, we need to setup 
repositories and also remote management so then:

```
cat > /etc/network/interfaces << EOF
auto lo
iface lo inet loopback
EOF

setup-interfaces -a

echo -e "\n" | setup-dns 8.8.8.8

rc-service networking restart
```

Now we need to added normal repositories, do not use edge or testing, 
due we will setup all for stable usage and daily use:

```
cat > /etc/apk/repositories << EOF
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://dl-cdn.alpinelinux.org/alpine/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```

Now that repositories are set, we still need to property configure root, 
added minimal set of packages to provide similar behavior of other linuxes, 
and install some others need tools:

```
cat > /root/.cshrc << EOF
unsetenv DISPLAY || true
HISTCONTROL=ignoreboth
EOF

cp -f /root/.cshrc /root/.profile

apk del dropbear

apk add openssh openssh-doc readline readline-doc

apk add man man-pages lsof lsof-doc less less-doc nano nano-doc curl curl-doc

apk add attr dialog dialog-doc bash bash-doc bash-completion grep grep-doc

apk add util-linux pciutils usbutils binutils findutils readline

apk add util-linux-doc pciutils-doc usbutils-doc binutils-doc findutils-doc

export PAGER=less
```

Now at this point we have a console linux (no desktop graphical session) ready, 
each console can be access with hitting at same time keys `Ctrl`+`Alt`+`FX` 
where `X` can be 1 to 6 as we already know as function keys, by example, 
if we hit `Ctrl+Alt+F3` we will see a TTY3 console with "login" prompt; 
where can input the username and later after hit enter input the user password, 
if both are correrct a linux console prompt will display with `$` symbol 
ready to receive commands inputs.

# que hacer despeus de instalar:

Lease [Receta despues de instalar: configuracion y paquetes](../recetas/alpine-recetas-configuracion-y-paquetes-sistema.md).
