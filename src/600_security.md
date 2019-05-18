# Seguridad

## Autenticación en servidores por clave pública

Generar contraseñas para conexión servidores remotos:

~~~~
cd ~
ssh-keygen -b 4096 [-t dsa | ecdsa | ed25519 | rsa | rsa1]
cat .ssh/
~~~~

Solo resta añadir nuestra clave pública en el fichero
`authorized_keys` del servidor remoto.

~~~~
cat ~/.ssh/id_xxx.pub | ssh user@hostname 'cat >> .ssh/authorized_keys'
~~~~

[¿Cómo funciona esto?](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process)

## Claves gpg

`gpg --gen-key` Para generar nuestra clave.

* __Siempre__ hay que ponerle una fecha de expiración, la puedes cambiar más tarde.
* __Siempre__ hay que escoger la máxima longitud posible

## Seahorse

Para manejar todas nuestras claves con comodidad:
 
 `sudo apt install seahorse`
 
## Conexión a github con claves ssh

Usando este método podemos conectarnos a github sin tener que teclear
la contraseña en cada conexión.

### Claves ssh

Podemos echar un ojo a nuestras claves desde `seahorse` la aplicación
de gestión de claves que hemos instalado. También podemos ver las
claves que tenemos generadas:

~~~
ls -al ~/.ssh
~~~

En las claves listadas nuestras claves públicas aparecerán con
extensión `.pub`

También podemos comprobar que claves hemos añadido ya a nuestro agente
ssh con:

~~~
ssh-add -l
~~~

Para generar una nueva pareja de claves ssh:

~~~
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
~~~

Podremos dar un nombre distintivo a los ficheros de claves generados y
poner una contraseña adecuada a la clave. Si algún dia queremos
cambiar la contraseña:

~~~
ssh-keygen -p
~~~

Ahora tenemos que añadir nuestra clave ssh en nuestra cuenta de
github, para ello editamos con nuestro editor de texto favorito el
fichero `~/.ssh/id_rsa.pub` y copiamos el contenido integro. Después
pegamos ese contenido en el cuadro de texto de la web de github.

Para comprobar que las claves instaladas en github funcionan
correctamente:

~~~~
ssh -T git@github.com
Hi salvari! You've successfully authenticated, but GitHub does not provide shell access.
~~~~

Este mensaje indica que todo ha ido bien.

Ahora en los repos donde queramos usar ssh debemos cambiar el remote:

~~~~
git remote set-url origin git@github.com:$USER/$REPONAME.git
~~~~

## Signal

El procedimiento recomendado en la página oficial:

~~~~
curl -s https://updates.signal.org/desktop/apt/keys.asc | sudo apt-key add -
echo "deb [arch=amd64] https://updates.signal.org/desktop/apt xenial main" | sudo tee -a /etc/apt/sources.list.d/signal-xenial.list
sudo apt update && sudo apt install signal-desktop
~~~~

------------

__NOTA__: Parece que no funciona. Lo he instalado via _flatpack_

------------

## Lector DNI electrónico

Instalamos:

~~~~
sudo apt-get install pcscd pcsc-tools libccid
~~~~

Como root ejecutamos pcsc_scan:

~~~~
root@rasalhague:~# pcsc_scan 
PC/SC device scanner
V 1.4.23 (c) 2001-2011, Ludovic Rousseau <ludovic.rousseau@free.fr>
Compiled with PC/SC lite version: 1.8.11
Using reader plug'n play mechanism
Scanning present readers...
Waiting for the first reader...
~~~~

Si insertamos el lector veremos algo como esto:

~~~~
root@rasalhague:~# pcsc_scan 
PC/SC device scanner
V 1.4.23 (c) 2001-2011, Ludovic Rousseau <ludovic.rousseau@free.fr>
Compiled with PC/SC lite version: 1.8.11
Using reader plug'n play mechanism
Scanning present readers...
Waiting for the first reader...found one
Scanning present readers...
0: C3PO LTC31 v2 (11061005) 00 00

Wed Jan 25 01:17:20 2017
Reader 0: C3PO LTC31 v2 (11061005) 00 00
  Card state: Card removed,
~~~~

Si insertamos un DNI veremos que se lee la información de la tarjeta insertada:

~~~~
Reader 0: C3PO LTC31 v2 (11061005) 00 00
  Card state: Card inserted, 
~~~~

y mas rollo

Instalamos ahora el modulo criptográfico desde [este
enlace](https://www.dnielectronico.es/PortalDNIe/PRF1_Cons02.action?pag=REF_1112)


Y además:

~~~~
aptitude install pinentry-gtk2 opensc
~~~~
