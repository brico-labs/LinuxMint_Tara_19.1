# Sonido

## Spotify

Spotify instalado desde las opciones de Linux Mint

## Audacity

Añadimos ppa:

~~~~
sudo add-apt-repository ppa:ubuntuhandbook1/audacity
sudo apt-get update
sudo apt install audacity
~~~~

Instalamos también el plugin [Chris’s Dynamic Compressor
plugin](https://theaudacitytopodcast.com/chriss-dynamic-compressor-plugin-for-audacity/)

## Clementine

~~~~
sudo add-apt-repository ppa:me-davidsansome/clementine
sudo apt update
sudo apt install clementine
~~~~

# Video

## Shotcut

Nos bajamos la _AppImage_ para Linux desde la [página
web](https://www.shotcut.org/).

La dejamos en `~/apps/shotcut` y:

~~~~
cd
chmod 744 Shotcutxxxxxx.AppImage
./Shotcutxxxxxx.AppImage
~~~~

Al ejecutarla la primera vez ya se encarga la propia aplicación de
integrarse en nuestro sistema.

## kdenlive

Otra más en la que bajamos _appimage_, como siempre descargamos en
`~/app/kdenlive`, damos permisos de ejecución y lo probamos.

Este paquete _appimage_ no se integra en el sistema, tendremos que
crear una entrada de menú con _MenuLibre_.

## Grabación de screencast

### Vokoscreen y Kazam

Instalados desde los repos oficiales:

~~~~
sudo apt update
sudo apt install vokoscreen kazam
~~~~

# Fotografía

## Rawtherapee

Bajamos el AppImage desde la [página web](http://rawtherapee.com/) al
directorio `~/apps/rawtherapee`.

~~~~
cd
chmod 744 RawTherapeexxxxxx.AppImage
./RawTherapeexxxxxx.AppImage
~~~~

Al ejecutarla la primera vez ya se encarga la propia aplicación de
integrarse en nuestro sistema.


## Darktable

Instalamos ppa:

~~~~
sudo add-apt-repository ppa:pmjdebruijn/darktable-release

sudo apt update && sudo apt install darktable
~~~~

-----

__OJO__: Do not install a more recent version of Lensfun using
another PPA as it will likely cause issues due the API changes. The
lensfun package for Xenial on this PPA already has new lenses patched
in.

-----
