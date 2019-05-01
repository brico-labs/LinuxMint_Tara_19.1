# Documentación

## Vanilla LaTeX

El LaTeX de Debian está un poquillo anticuado, si se quiere usar una versión reciente hay que aplicar este truco.

~~~~
cd ~
mkdir tmp
cd tmp
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
tar xzf install-tl-unx.tar.gz
cd install-tl-xxxxxx
~~~~

La parte xxxxxx varía en función del estado de la última versión de LaTeX disponible.

~~~~
sudo ./install-tl
~~~~

Una vez lanzada la instalación podemos desmarcar las opciones que
instalan la documentación y las fuentes. Eso nos obligará a consultar
la documentación on line pero ahorrará practicamente el 50% del
espacio necesario. En mi caso sin doc ni src ocupa 2,3Gb

~~~~
mkdir -p /opt/texbin
sudo ln -s /usr/local/texlive/2018/bin/x86_64-linux/* /opt/texbin
~~~~

Por último para acabar la instalación añadimos `/opt/texbin` al
_PATH_. Para _bash_ y _zsh_ basta con añadir al fichero `~/.profile`
las siguientes lineas:

~~~~
# adds texlive to my PATH
if [ -d "/opt/texbin" ] ; then
    PATH="$PATH:/opt/texbin"
fi
~~~~

En cuanto a _fish_ (si es que lo usas, claro) tendremos que modificar
(o crear) el fichero `~/.config/fish/config.fish` y añadir la
siguiente linea:

~~~~
set PATH $PATH /opt/texbin
~~~~

### Falsificando paquetes

Ya tenemos el _texlive_ instalado, ahora necesitamos que el gestor de
paquetes sepa que ya lo tenemos instalado.

~~~~
sudo apt install equivs --no-install-recommends
mkdir -p /tmp/tl-equivs && cd /tmp/tl-equivs
equivs-control texlive-local
~~~~

Alternativamente para hacerlo más fácil podemos descargarnos un
fichero `texlive-local`ya preparado, ejecutando:

~~~~
wget http://www.tug.org/texlive/files/debian-equivs-2018-ex.txt
/bin/cp -f debian-equivs-2018-ex.txt texlive-local
~~~~

Editamos la versión (si queremos) y procedemos a generar el paquete
_deb_.

~~~~
equivs-build texlive-local
~~~~

El paquete que hemos generado tiene una dependencia: _freeglut3_, hay
que instalarla previamente.

~~~~
sudo apt install freeglut3
sudo dpkg -i texlive-local_2018-1_all.deb
~~~~

Todo listo, ahora podemos instalar cualquier paquete debian que
dependa de _texlive_ sin problemas de dependencias, aunque no hayamos
instalado el _texlive_ de Debian.

### Fuentes

Para dejar disponibles las fuentes opentype y truetype que vienen con texlive para el resto de aplicaciones:

~~~~
sudo cp $(kpsewhich -var-value TEXMFSYSVAR)/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf
gksudo gedit /etc/fonts/conf.d/09-texlive.conf
~~~~

Borramos la linea:

~~~~
<dir>/usr/local/texlive/2018/texmf-dist/fonts/type1</dir>
~~~~

Y ejecutamos:

~~~~
sudo fc-cache -fsv
~~~~

Actualizaciones
Para actualizar nuestro _latex_ a la última versión de todos los paquetes:

~~~~
sudo /opt/texbin/tlmgr update --self
sudo /opt/texbin/tlmgr update --all
~~~~

También podemos lanzar el instalador gráfico con:

~~~~
sudo /opt/texbin/tlmgr --gui
~~~~

Para usar el instalador gráfico hay que instalar previamente:

~~~~
sudo apt-get install perl-tk --no-install-recommends
~~~~

Lanzador para el actualizador de _texlive_:

~~~~
mkdir -p ~/.local/share/applications
/bin/rm ~/.local/share/applications/tlmgr.desktop
cat > ~/.local/share/applications/tlmgr.desktop << EOF
[Desktop Entry]
Version=1.0
Name=TeX Live Manager
Comment=Manage TeX Live packages
GenericName=Package Manager
Exec=gksu -d -S -D "TeX Live Manager" '/opt/texbin/tlmgr -gui'
Terminal=false
Type=Application
Icon=system-software-update
EOF
~~~~

## Tipos de letra

Creamos el directorio de usuario para tipos de letra:

~~~~
mkdir ~/.local/share/fonts
~~~~

## Fuentes Adicionales

Un par de fuentes las he descargado de internet y las he almacenado en
el directorio de usuario para los tipos de letra: `~/.local/share/fonts`

* [Ubuntu](https://design.ubuntu.com/font/) (La uso en documentación)
* [Mensch](https://robey.lag.net/downloads/mensch.ttf) (Esta es la que yo uso para programar.)

Además he clonado el repo [_Programming
Fonts_](https://github.com/ProgrammingFonts/ProgrammingFonts) y enlazado algunas fuentes (Hack y Menlo)

~~~~
cd ~/wherever
git clone https://github.com/ProgrammingFonts/ProgrammingFonts
cd ~/.local/share/fonts
ln -s ~/wherever/ProgrammingFonts/Hack .
ln -s ~/wherever/ProgrammingFonts/Menlo .
~~~~

## Pandoc

_Pandoc_ es un traductor entre formatos de documento. Está escrito en
Python y es increiblemente útil. De hecho este documento está escrito
con _Pandoc_.

Instalado el _Pandoc_ descargando paquete deb desde [la página web del
proyecto](http://pandoc.org/installing.html).

Además descargamos plantillas adicionales desde [este
repo](https://github.com/jgm/pandoc-templates) ejecutando los
siguientes comandos:

~~~~
mkdir ~/.pandoc
cd ~/.pandoc
git clone https://github.com/jgm/pandoc-templates templates
~~~~

## Calibre

La mejor utilidad para gestionar tu colección de libros electrónicos.

Ejecutamos lo que manda la página web:

~~~~
sudo -v && wget -nv -O- https://raw.githubusercontent.com/kovidgoyal/calibre/master/setup/linux-installer.py \
| sudo python -c "import sys; main=lambda:sys.stderr.write('Download failed\n'); exec(sys.stdin.read()); main()"
~~~~

Para usar el calibre con el Kobo Glo:

* Desactivamos todos los plugin de Kobo menos el Kobo Touch Extended
* Creamos una columna MyShelves con identificativo #myshelves
* En las opciones del plugin:
    * En la opción Collection columns añadimos las columnas series,#myshelves
    * Marcamos las opciones Create collections y Delete empy collections
    * Marcamos _Modify CSS_
    * Update metadata on device y Set series information

Algunos enlaces útiles:

* (https://github.com/jgoguen/calibre-kobo-driver)
* (http://www.lectoreselectronicos.com/foro/showthread.php?15116-Manual-de-instalaci%C3%B3n-y-uso-del-plugin-Kobo-Touch-Extended-para-Calibre)
* (http://www.redelijkheid.com/blog/2013/7/25/kobo-glo-ebook-library-management-with-calibre)
* (https://www.netogram.com/kobo.htm)

## Scribus

Scribus es un programa libre de composición de documentos. con Scribus
puedes elaborar desde los folletos de una exposición hasta una revista
o un poster.

Para tener una versión más actualizada instalamos:

~~~~
sudo add-apt-repository ppa:scribus/ppa
sudo apt update
sudo apt install scribus scribus-ng scribus-template scribus-ng-doc
~~~~

### Cambiados algunos valores por defecto

He cambiado los siguientes valores en las dos versiones, non están
exactamente en el mismo menú pero no son díficiles de encontrar:

* Lenguaje por defecto: __English__
* Tamaño de documento: __A4__
* Unidades por defecto: __milimeters__
* Show Page Grid: __Activado__
* Dimensiones de la rejilla:
    * Mayor: __30 mm__
    * Menor: __6mm__
* En opciones de salida de _pdf_ indicamos que queremos salida a
  impresora y no a pantalla. Y también que no queremos _spot colors_
  (que serían solo para ciertas impresoras industriales).
  
Siempre se puede volver a los valores por defecto sin mucho problema
(hay una opción para ello)

Referencia [aquí](https://www.youtube.com/watch?v=3sEoYZGABQM&list=PL3kOqLpV3a67b13TY3WxYVzErYUOLYekI)

### Solucionados problemas de _hyphenation_

_Scribus_ no hacia correctamente la separación silábica en castellano, he instalado los paquetes:

* hyphen-es
* hyphen-gl

Y ahora funciona correctamente.

