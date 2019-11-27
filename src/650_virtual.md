# Virtualizaciones y contenedores

## Instalación de _virtualBox_

* Descargamos el paquete correspondiente (Ubuntu 18.04) desde [la
  página web](https://www.virtualbox.org/wiki/Linux_Downloads).
* Descargamos también el [VirtualBox Extension Pack
  ](https://www.virtualbox.org/wiki/Downloads)
* Instalamos el paquete _deb_ de VirtualBox con `sudo dpkg -i paquete`
* Instalamos el _Extension Pack_ desde la propia aplicación de
  virtualbox (_File::Preferences::Extensions_)
* Añadimos nuestro usuario (o usuarios) al grupo `vboxusers` con `sudo
  gpasswd -a user vboxusers`
  
## qemu

Instalamos desde el repo oficial:

~~~~
sudo apt install qemu-kvm qemu virt-manager virt-viewer libvirt-bin
~~~~


## Docker

Tenemos que añadir el repositorio correspondiente a nuestra
distribución:

~~~~
# Be safe
sudo apt remove docker docker-engine docker.io
sudo apt autoremove
sudo apt update

# Install pre-requisites
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Import the GPG key

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Next, point the package manager to the official Docker repository

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(. /etc/os-release; echo "$UBUNTU_CODENAME") stable"

# Update the package database

sudo apt update
#

apt-cache policy docker-ce

sudo apt install docker-ce

sudo gpasswd -a salvari docker
~~~~
