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

## Posible problema con modemmanager y micros programables

Programando el _Circuit Playground Express_ con el _Arduino IDE_ tenía
problemas continuos para hacer los _uploads_, al parecer el servicio
_ModemManager_ es el culpable, se pasa todo el tiempo capturando los
nuevos puertos serie por que considera que todo es un modem.

Una prueba rápida para comprobarlo: `sudo systemctl stop ModemManager`

Con esto funciona todo bien, pero en el siguiente arranque volvera a
cargarse.

Para dar una solución definitiva se puede programar una regla para
impedir que el _ModemManager_ capture el puerto con un dispositivo

Creamos un fichero con permisos de `root` en el directorio
`/etc/udev/rules.d` que llamaremos: `99-arduino.rules`

Dentro de ese fichero especificamos los codigos VID/PID que se deben ignorar:

~~~~
# for arduino brand, stop ModemManager grabbing port
ATTRS{idVendor}=="2a03", ENV{ID_MM_DEVICE_IGNORE}="1"
# for sparkfun brand, stop ModemManager grabbing port
ATTRS{idVendor}=="1b4f", ENV{ID_MM_DEVICE_IGNORE}="1"
~~~~

Save the rules file (as root) and reboot the PC. Arduino IDE then works great with the Pro Micro.

Ojo que si tienes SystemV no va a funcionar.

https://starter-kit.nettigo.eu/2015/serial-port-busy-for-avrdude-on-ubuntu-with-arduino-leonardo-eth/

https://www.codeproject.com/Tips/349002/Select-a-USB-Serial-Device-via-its-VID-PID

## Programar los nanos con chip ch340 o ch341

Linux mapea el chip correctamente en un puerto `/dev/ttyUSB0` y con
eso basta, que no te lien con el cuento de "drivers para linux"

Todo lo que hace falta es configurar correctamente el _Arduino IDE_,
hay que escoger:

~~~~
Board: "Arduino Nano"
Processor: "ATmega168"
Port: "/dev/ttyUSB0"
~~~~

Y ya funciona todo.
