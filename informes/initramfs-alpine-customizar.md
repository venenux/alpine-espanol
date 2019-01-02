
En alpine en muchos hardware falla, solo ver un ejemplo este bug:
https://bugs.alpinelinux.org/issues/4216 este bug fue cerrado 
sin poder terminar de verificarse (obvio el reportador se canso)

Para poder arreglar estos fallos necesitamos "adicionar" al arranque 
modulos especificos de el hardware, obvimente de el kernel customizado, 
por ejemplo es bien conocido que desde Alpine 3.7 falla en laptops HP serie c7XX.

En este documento se enseñara como alterar a mano el initramfs que 
es la imagen inicial del kernel al cargar con grub (alpine usa syslinux)

### Initramfs en compresion CPIO+GZ:

* montar tu raiz o hacerle chroot (WIP explicacion)

* Descomprimir la imagen initramfs:

```
bash
mkdir -p /usr/src/kernelmisc
cd /usr/src/kernelmisc
rm -fr /usr/src/kernelmisc/*
cp /boot/initramfs-`uname -r | cut -d'-' -f3` /usr/src/kernelmisc/initramfs-last
zcat initramfs-last | cpio -idmv
rm initramfs-last
```

* Indagar dentro y colocar lo que se desee este disponible al arrancar 
(WIP sugerencias para HP laptops y IBM thinkpads que fallan siempre)

* Volver comprimir y generar la imagen initramfs:

```
cd /usr/src/kernelmisc/
mv -f /boot/initramfs-`uname -r | cut -d'-' -f3` /boot/initramfs-`uname -r | cut -d'-' -f3`.bck
find . | cpio -o -c | gzip -9 > /boot/initramfs-`uname -r | cut -d'-' -f3`
```

**IMPORTANTE** si ha compilado su propio kernel y la imagen esta en "xz" estos comandos 
no funcionaran, usar los siguientes.

### Initramfs en compresion CPIO+XZ:

WIP no he tenido un alpine con xz es que como casi nadie lo usa...
