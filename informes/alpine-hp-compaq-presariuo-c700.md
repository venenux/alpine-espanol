# Alpine en HP presario C700la

Instalacion y experiencia de alpine en Compaq HP Presario c700la (sirve a todos los c7XX)

## Apreciacion equipo

Es un equipo muy recomendado para cualquier trabajo, con un unico (pero terrible) defecto 
de mal diseño de hardware en suspension (usb camara se cuelga y mala ubicacion de ram).
**Si se trabaja en una oficina con aire acondicionado, este equipo es recomendado para toda tarea, pero** 
si se trabaja mucho en movimiento no se recomienda.

#### Pros:

* Es comodo, teclas grandes y bien ubicadas
* Buena calidad de ensamblado, pantalla y perifericos
* En su momento la relacion precio fue de las mejores
* Puertos de expansion todo a los lados, se puede colocr en las piernas sin problemas

#### Contras:

* Los puertos extras (usb,cdrom,video) estan ubicados solo al los lados
* Facil de calentar si agarra mucho polvo
* Falla en suspension para reestablecer los dispositivos, especialmente la camara.

## Apreciacion linux

A pesar de la mala relacion con linux de HP y Compaq, estos equipos C7XX son muy buenos 
con linux, recomendables a pesar de su gran fallo con la camara y otros dispositivos internos USB.

* Kernel minimo 2.6.34 para soporte completo, recomendado 3.6.12, no mas alla de 3.17 fallara
* Falla en suspension en reconectar los perifericos internos, especialmente la camara.

## lspci

```
00:00.0 Host bridge: Intel Corporation Mobile PM965/GM965/GL960 Memory Controller Hub (rev 03)
00:02.0 VGA compatible controller: Intel Corporation Mobile GM965/GL960 Integrated Graphics Controller (primary) (rev 03)
00:02.1 Display controller: Intel Corporation Mobile GM965/GL960 Integrated Graphics Controller (secondary) (rev 03)
00:1b.0 Audio device: Intel Corporation 82801H (ICH8 Family) HD Audio Controller (rev 03)
00:1c.0 PCI bridge: Intel Corporation 82801H (ICH8 Family) PCI Express Port 1 (rev 03)
00:1d.0 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #1 (rev 03)
00:1d.1 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #2 (rev 03)
00:1d.2 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #3 (rev 03)
00:1d.7 USB controller: Intel Corporation 82801H (ICH8 Family) USB2 EHCI Controller #1 (rev 03)
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev f3)
00:1f.0 ISA bridge: Intel Corporation 82801HM (ICH8M) LPC Interface Controller (rev 03)
00:1f.1 IDE interface: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) IDE Controller (rev 03)
00:1f.2 SATA controller: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) SATA Controller [AHCI mode] (rev 03)
00:1f.3 SMBus: Intel Corporation 82801H (ICH8 Family) SMBus Controller (rev 03)
01:00.0 Ethernet controller: Atheros Communications Inc. AR242x / AR542x Wireless Network Adapter (PCI-Express) (rev 01)
02:01.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL-8139/8139C/8139C+ (rev 10)
```

## Network

```
01:00.0 Ethernet controller: Atheros Communications Inc. AR242x / AR542x Wireless Network Adapter (PCI-Express) (rev 01)
02:01.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL-8139/8139C/8139C+ (rev 10)
```

#### Wired network

REalteck, bien soportado en cualquier alpine.

#### Wireless Network

Soportado desde alpine 3.2 con firmwares.

## Audio

Bien soportado ALSA no requiere de configuracion, OSS4 tampoco.

## Video

Placa de video estandar no discrete, con 3 salidas, VGA (monitor compuesto), LDVS (pantalla LSD), STVO (Video compuesto)

## Camara

Un dolor de cabeza si se suspende se bloquea, tambien si levanta chromium trata acceder camara y a veces se bloquea.
No hay manera de solucionarlo.

`uvccapture error failed to set probe control`

## ACPI

Gestiona todo perfecto desde linux 3.2.34 cualquier alpine lo soporta, incluso las teclas de funcion.

# Resumen

**Pros** Perfecta para programar o jugar por su teclado, si tiene conexcion directa por cable, 
es muy silenciosa y su procesador aguanta bien (el video es el que hoy dia sera algo flojo).

**Contra** Inutil si se emplea como distro de uso ya que la configuracion de la inalambrica 
es un dolor de cabeza si entra en un sitio que requiere de rapida configuracion.
