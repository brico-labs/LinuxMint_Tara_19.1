# Utilidades para mapas y cartografía

## josm

Creamos el fichero `/etc/apt/sources.list.d/josm.list` que contiene la linea:

~~~~
deb https://josm.openstreetmap.de/apt artful universe
~~~~

Descargamos y añadimos la clave gpg:

~~~~
wget -q https://josm.openstreetmap.de/josm-apt.key -O- | sudo apt-key add -
~~~~

~~~~
sudo add-apt-repository "deb [arch=amd64] https://josm.openstreetmap.de/apt $(. /etc/os-release; echo "$UBUNTU_CODENAME") universe"
~~~~

Y ahora procedemos a la instalación:

~~~~
sudo apt update
sudo apt install openjfx josm 
~~~~

También podemos instalar la versión "nightly" pero tendréis actualizaciones diarias:

~~~~
sudo apt josm-latest
~~~~

Ya estamos listos para editar Open Street Map offline.

## MOBAC

Bajamos el paquete desde [la página
web](http://mobac.sourceforge.net/) y descomprimimos en `~/apps/mobac`
como de costumbre nos creamos una entrada de menú con _MenuLibre_.

Conviene bajarse wms adicionales para MOBAC y leerse [la
wiki](http://mobac.sourceforge.net/wiki/index.php/Custom_XML_Map_Sources)

### Referencias

*[Cartografía digital] (https://digimapas.blogspot.com.es/2015/01/oruxmaps-vii-mapas-de-mobac.html)

## QGIS

Añadimos la clave gpg:

~~~~
wget -q  https://qgis.org/downloads/qgis-2017.gpg.key -O- | sudo apt-key add -
~~~~

Ejecutamos:

~~~~
sudo add-apt-repository "deb [arch=amd64] https://qgis.org/debian $(. /etc/os-release; echo "$UBUNTU_CODENAME") main"
~~~~

E instalamos como siempre

~~~~
sudo apt update
sudo apt install qgis
~~~~

### Referencias

* [Conectar WMS con QGIS](https://mappinggis.com/2015/09/como-conectar-con-servicios-wms-y-wfs-con-arcgis-qgis-y-gvsig/)
* [Importar OSM en QGIS](https://www.altergeosistemas.com/blog/2014/03/28/importando-datos-de-osm-en-qgis-2/)
* [Learn OSM](http://learnosm.org/es/osm-data/osm-in-qgis/)
* [QGIS Tutorials](http://www.qgistutorials.com/es/docs/downloading_osm_data.html)
