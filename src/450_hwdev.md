# Desarrollo hardware

## Arduino IDE

Bajamos los paquetes de la página [web](https://www.arduino.cc),
descomprimimimos en _~/apps/arduino_.

La distribución del IDE incluye ahora un fichero que se encarga de
hacer la integración del IDE en los menús de Linux.

Hay que añadir nuestro usuario al grupo `dialout`:

~~~~
sudo gpasswd --add <usrname> dialout
~~~~

### Añadir biblioteca de soporte para Makeblock

Clonamos el
[repo oficial en github](https://github.com/Makeblock-official/Makeblock-Libraries).

Una vez que descarguemos las librerias es necesario copiar el
directorio `Makeblock-Libraries/makeblock` en nuestro directorio de
bibliotecas de Arduino. En mi caso `~/Arduino/libraries/`.

Una vez instaladas las bibliotecas es necesario reiniciar el IDE
Arduino si estaba arrancado. Podemos ver si se ha instalado
correctamente simplemente echando un ojo al menú de ejemplos en el
IDE, tendríamos que ver los ejemplos de _Makeblock_.

Un detalle importante para programar el Auriga-Me es necesario
seleccionar el micro Arduino Mega 2560 en el IDE Arduino.


## Pinguino IDE

-------------------------------------------------------------------------------
__Nota__: Pendiente de instalar
-------------------------------------------------------------------------------

Tenemos el paquete de instalación disponible en su página
[web](http://pinguino.cc/download.php)

Ejecutamos el programa de instalación. El programa descargará los
paquetes Debian necesarios para dejar el IDE y los compiladores
instalados.

Al acabar la instalación he tenido que crear el directorio
_~/Pinguino/v11_, parece que hay algún problema con el programa de
instalación y no lo crea automáticamente.

El programa queda correctamente instalado en _/opt_ y arranca
correctamente, habrá que probarlo con los micros.

## KiCAD

En la [página web del
proyecto](http://kicad-pcb.org/download/linux-mint/) nos recomiendan
el ppa a usar para instalar la última versión estable:

~~~~
sudo add-apt-repository --yes ppa:js-reynaud/kicad-5
sudo apt-get update
sudo apt-get install kicad
sudo apt install kicad-footprints kicad-libraries kicad-packages3d kicad-symbols kicad-templates
~~~~

Paciencia, el paquete `kicad-packages3d` tarda un buen rato en descargarse.

Algunas librerías alternativas:

* [Freetronics](https://github.com/freetronics/freetronics_kicad_library)
  una libreria que no solo incluye Shield para Arduino sino una
  completa colección de componentes que nos permitirá hacer proyectos
  completos. [Freetronics](http://www.freetronics.com) es una especie
  de BricoGeek australiano, publica tutoriales, vende componentes, y
  al parecer mantiene una biblioteca para KiCAD. La biblioteca de
  Freetronics se mantiene en un repo de github. Lo suyo es
  incorporarla a cada proyecto, por que si la actualizas se pueden
  romper los proyectos que estes haciendo.
* [eklablog](http://meta-blog.eklablog.com/kicad-librairie-arduino-pretty-p930786)
  Esta biblioteca de componentes está incluida en el github de KiCAD, así que
  teoricamente no habría que instalarla en nuestro disco duro.
  
## Analizador lógico

Mi analizador es un OpenBench de
Seedstudio,
[aquí hay mas info](http://dangerousprototypes.com/docs/Open_Bench_Logic_Sniffer)

### Sigrok

Instalamos __Sigrok__, simplemente desde los repos de Debian:

~~~~{bash}
sudo aptitude install sigrok
~~~~

Al instalar __Sigrok__ instalamos también __Pulseview__.

Si al conectar el analizador, echamos un ojo al
fichero _syslog_ vemos que al conectarlo se mapea en un puerto tty.

Si arrancamos __Pulseview__ (nuestro usuario tiene que estar incluido
en el grupo _dialout_), en la opción _File::Connect to device_,
escogemos la opción _Openbench_ y le pasamos el puerto.  Al pulsar la
opción _Scan for devices_ reconoce el analizador correctamente como un
_Sump Logic Analyzer_.

### Sump logic analyzer

Este es el software recomendado para usar con el analizador.

Descargamos el paquete de la [página del
proyecto](https://www.sump.org), o más concretamente de [esta
página](https://www.sump.org/projects/analyzer/) y descomprimimos en
_~/apps_.

Instalamos las dependencias:

~~~~{bash}
sudo apt install librxtx-java
~~~~

Editamos el fichero _~/apps/Logic Analyzer/client/run.sh_ y lo dejamos
así:

~~~~
#!/bin/bash

# java -jar analyzer.jar $*
java -cp /usr/share/java/RXTXcomm.jar:analyzer.jar org.sump.analyzer.Loader
~~~~

Y ya funciona.

### OLS

-------------------------------------------------------------------------------
__Nota__: Pendiente de instalar
-------------------------------------------------------------------------------

[Página oficial](https://www.lxtreme.nl/ols/)

## IceStudio

Instalamos dependencias con `sudo apt install xclip`

Bajamos el _AppImage_ desde el [github de
IceStudio](https://github.com/FPGAwars/icestudio) y lo dejamos en
`~/apps/icestudio`

## PlatformIO

Nos bajamos el paquete para instalar el [Atom IDE](https://atom.io/)

Instalamos el paquete `.deb` que nos hemos bajado:

~~~~
sudo apt deb atom-amd64.deb
~~~~

Instalamos `clang` (una dependencia de PlatformIO)

~~~~
sudo apt install clang
~~~~

Completamos la instalación del paquete desde el Atom siguiendo las
instrucciones
[aquí](https://platformio.org/get-started/ide?install=atom).

Para poder usar _platformio_ sin usar _Atom_ añadimos la siguiente
linea al fichero `.profile`:

~~~~
export PATH=$PATH:~/.platformio/penv/bin
~~~~

* [Referencia](https://docs.platformio.org/en/latest/installation.html#piocore-install-shell-commands)


## RepRap

### OpenScad

El OpenSCAD disponible en los orígenes de software parece ser la
última estable. Así que instalamos con `apt`:

~~~~
sudo apt install openscad
~~~~

### Slic3r

Descargamos la estable desde la [página web](https://dl.slic3r.org) y
como de costumbre descomprimimos en `~/apps` y creamos un lanzador con
_MenuLibre_

### Slic3r Prusa Edition

Una nueva versión del clásico _Slic3r_ con muchas mejoras. Descargamos
la _appimage_ desde la [página
web](https://www.prusa3d.com/slic3r-prusa-edition/) y ya sabeis,
descomprimir en `~/apps` y dar permisos de ejecución.

### ideaMaker

Una aplicación más para generar gcode con muy buena pinta, tenemos el
paquete _deb_ disponible en su [página
web](https://www.raise3d.com/pages/ideamaker). Instalamos con el
gestor de software.

### Ultimaker Cura

Descargamos el _AppImage_ desde la [página
web](https://github.com/Ultimaker/Cura/releases)

### Pronterface

Seguimos las instrucciones para Ubuntu Bionic:

Instalamos las dependencias:

~~~~
sudo apt install python3-serial python3-numpy cython3 python3-libxml2 \
python3-gi python3-dbus python3-psutil python3-cairosvg libpython3-dev \
python3-appdirs python3-wxgtk4.0

pip3 install --user piglet
~~~~

Clonamos el repo:

~~~~
cd ~/apps
git clone https://github.com/kliment/Printrun.git
~~~~

Y ya lo tenemos todo listo para ejecutar.

## Cortadora de vinilos

### Inkcut

Instalado en un entorno virtual:

~~~~{bash}
mkvirtualenv -p `which python3` inkcut

sudo apt install libxml2-dev libxslt-dev libcups2-dev

pip install PyQt5

pip install inkcut
~~~~

### Plugin para inkscape

Instalamos dependencias:

~~~~{bash}
pip install python-usb
~~~~

Instalamos el fichero `.deb` desde la web
<https://github.com/fablabnbg/inkscape-silhouette/releases>
