
Either way, this section describes, step by step, how to get 
and install usage of main setup-* utilities, as well as explanations 
for a complete install on a DELL VOSTRO common computer, 
a DELL Vostro 230 was used specifically for this document:

# 0. Source media

First download a source media, for computers only `x86` and `x86_64` 
are valids images from https://alpinelinux.org/downloads/, the 
recommended image are `extended` due comes with need tools 
and only are 100Mb.

For all users no cares from what operating system comes, 
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
in this case USB drive was used, rest of this document makes no sense which media was used.

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
due today Alpine not have a good remote x11 connection that x11vnc.. 

The remote management cannot be done with root directly by default, due ssh 
security, so we need to setup an remote connection account, we due VenenuX 
guides compatibility called "daru" and only will be able to connect to made "su" 
once connected:

```
adduser --uid 888 --home /opt/daru --shell /bin/ash daru

rm /opt/daru/*

cat > /opt/daru/.logout << EOF
history -c
/bin/rm -f /opt/daru/.mysql_history
/bin/rm -f /opt/daru/.history
/bin/rm -f /opt/daru/.bash_history
EOF

cat > /opt/daru/.cshrc << EOF
unsetenv DISPLAY
set autologout = 30
set prompt = "$ "
set history = 0
set ignoreeof
EOF

cat > /opt/daru/.bashrc << EOF
unsetenv DISPLAY
set autologout = 30
set prompt = "$ "
set history = 0
set ignoreeof
EOF
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

Now we need to added normal repositories, donot use edge or testing, 
due we will setup all for stable usage adn daily use:

```
cat > /etc/apk/repositories << EOF
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/main
http://mirror.math.princeton.edu/pub/alpinelinux/v$(cat /etc/alpine-release | cut -d'.' -f1,2)/community
EOF

apk update
```

now that repositories are set, we sill added minimal set of packages 
to provide similar behaviour of other linuxes:

```
apk add attr dialog dialog-doc bash bash-doc bash-completion grep grep-doc

apk add util-linux util-linux-doc pciutils usbutils binutils findutils readline

apk add man man-pages lsof lsof-doc less less-doc nano nano-doc curl curl-doc

export PAGER=less
```


