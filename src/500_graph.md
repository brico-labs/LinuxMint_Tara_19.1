# Aplicaciones de gráficos

## LibreCAD

Diseño en 2D

~~~~
sudo apt install librecad
~~~~

## FreeCAD

Añadimos el ppa de la última estable:

~~~~
sudo add-apt-repository ppa:freecad-maintainers/freecad-stable
sudo apt update
sudo install freecad
~~~~

-----

__NOTA:__ the ccx package brings CalculiX support to the FEM
workbench, and needs to be installed separately.

----

## Inkscape

El programa libre para creación y edición de gráficos vectoriales.

~~~~
sudo apt install inkscape
~~~~

## Gimp

El programa para edición y retocado de imágenes.

Gimp ya estaba instalado, pero no es la última versión, prefiero tener
la última así que:

~~~~
sudo apt remove gimp gimp-data
sudo add-apt-repository ppa:otto-kesselgulasch/gimp
sudo apt update
sudo apt upgrade
sudo apt install gimp-texturize gimp-data-extras \
gimp-gap gmic gimp-gmic gimp-python
~~~~

### Plugins de Gimp

#### resynthesizer

Descargamos el plugin desde
[aquí](https://github.com/bootchk/resynthesizer) y descomprimimos el
fichero en `~/.config/GIMP/2.10/plug-ins`

Tenemos que asegurarnos que los fichero _python_ son ejecutables:

~~~~
chmod 755 ~/.config/GIMP/2.10/plug-ins/*.py
~~~~

## Krita

La versión disponible en orígenes de software está bastante por detrás
de la disponible en la web. Basta con descargar el _Appimage_ desde la
[página web](https://krita.org)

Lo copiamos a `~/apps/krita` y creamos un lanzador con `Menulibre`

## MyPaint

Tenemos disponible la versión 1.2.0 en los orígenes de software.
1.2.0, que no difiere mucho de la 1.2.1 disponible en el

Desde el [github](https://github.com/mypaint/) tenemos disponible la
última versión en formato _appimage_. La descargamos la dejamos en
`~/apps` y creamos un acceso con _Menulibre_, como siempre.


## Alchemy



## Shutter

Un programa de capturas de pantalla.

~~~~
sudo apt install libgoo-canvas-perl
sudo apt install shutter
~~~~

## dia

Un programa para crear diagramas

~~~~
sudo apt install dia dia-shapes gsfonts-x11
~~~~

## Blender

Bajamos el Blender linkado estáticamente de [la página
web](https://www.blender.org) y lo descomprimimos en `~/apps/blender`.

## Structure Synth

Instalado desde repos, junto con sunflow para explorar un poco.

~~~~
sudo apt install structure-synth sunflow
~~~~

## Heron animation

Descargamos el programa desde [su página
web](https://heronanimation.brunolefevre.net/) y como siempre
descomprimimos en `~/apps/heron`

## Stopmotion

Primero probamos el del repo.

## Instalación del driver digiment para tabletas gráficas Huion

Ejecutamos los siguientes pasos:

~~~~
sudo git clone https://github.com/DIGImend/digimend-kernel-drivers.git /usr/src/digimend-6
sudo dkms build digimend/6
sudo dkms install digimend/6
~~~~

Para comprobar:

~~~~
xinput --list
dkms status
~~~~

Referencia: 

* [Aquí](https://davidrevoy.com/article331/setup-huion-giano-wh1409-tablet-on-linux-mint-18-1-ubuntu-16-04)

