# Recetas variadas

## Solucionar problemas de menús duplicados usando menulibre

En el directorio `~/.config/menus/applications-merged` borramos todos
los ficheros que haya.

## Formatear memoria usb 

"The driver descriptor says the physical block size is 2048 bytes, but Linux says it is 512 bytes."

Este comando borró todas las particiones de la memoria:

`sudo dd if=/dev/zero of=/dev/sdd bs=2048 count=32 && sync`

I'm assuming your using gparted.

First delete whatever partitions you can...just keep pressing ignore.

There will be one with a black outline...you will have to unmount
it...just right click on it and unmount.

Again you will have to click your way through ignore..if fix is an
option choose it also.

Once all this is done... you can select the device menu and choose new
partition table.

Select MSdos

Apply and choose ignore again.

Once it's done it show it's real size.

Next you can format the drive to whichever file system you like.

It's a pain in the behind this way, but it's the only way I get it
done..I put live iso's on sticks all the time and have to remove
them. I get stuck going through this process every time.

## Copiar la clave pública ssh en un servidor remoto

`cat /home/tim/.ssh/id_rsa.pub | ssh tim@just.some.other.server 'cat >> .ssh/authorized_keys'`

O también:

`ssh-copy-id -i ~/.ssh/id_rsa.pub username@remote.server`

## ssh access from termux
<https://linuxconfig.org/ssh-into-linux-your-computer-from-android-with-termux>

## SDR instalaciones varias

Vamos a trastear con un dispositivo [RTL-SDR.com](https://www.rtl-sdr.com/). 

Tenemos un montón de información en el blog de [SDR
Galicia](https://sdrgal.wordpress.com/) y tienen incluso una guia de
instalación muy completa, pero yo voy a seguir una guía un poco menos
ambiciosa, por lo menos hasta que pueda hacer el curso que imparten
ellos mismos (SDR Galicia)

La guía en cuestión la podemos encontrar
[aquí](https://ranous.wordpress.com/rtl-sdr4linux/)

Seguimos los pasos de instalación:

* La instalación de `git`, `cmake` y `build-essential` ya la tengo hecha.

~~~~
sudo apt-get install libusb-1.0-0-dev
~~~~
