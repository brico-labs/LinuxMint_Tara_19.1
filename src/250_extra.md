# Utilidades

## htop

~~~~
sudo apt install htop
~~~~

## gparted

Instalamos _gparted_ para poder formatear memorias usb

`sudo apt install gparted`

## wkhtmltopdf

~~~~
sudo apt install wkhtmltopdf
~~~~

# Internet

# Rclone

Instalamos desde la página web, siempre que te fies obviamente.

~~~~
curl https://rclone.org/install.sh | sudo bash
~~~~

## Recetas rclone

Copiar directorio local en la nube:

~~~~
rclone copy /localdir hubic:backup -vv
~~~~

Si queremos ver el directorio en la web de Hubic tenemos que copiarlo
en _default_:

~~~~
rclone copy /localdir hubic:default/backup -vv
~~~~

Sincronizar una carpeta remota en local:

~~~~
rclone sync hubic:directorio_remoto /home/salvari/directorio_local -vv
~~~~


## Referencias

* [Como usar rclone (blogdelazaro)](https://elblogdelazaro.gitlab.io//articles/rclone-sincroniza-ficheros-en-la-nube/)
* [y con cifrado (blogdelazaro)](https://elblogdelazaro.gitlab.io//articles/rclone-cifrado-de-ficheros-en-la-nube/)
* [Documentación](https://rclone.org/docs/)

# Tareas

## hamster-indicator

Tan fácil como:

~~~~
sudo apt install hamster-indicator
~~~~
