# Desarrollo software

## Paquetes esenciales

Estos son los paquetes esenciales para empezar a desarrollar software en Linux.

~~~~
sudo apt install build-essential checkinstall make automake cmake autoconf git git-core dpkg wget
~~~~

## Git

Control de versiones distribuido. Imprescindible. Para _Linux Mint_
viene instalado por defecto.

Configuración básica de git:

~~~~
git config --global ui.color auto
git config --global user.name "Pepito Pérez"
git config --global user.email "pperez@mikasa.com"

git config --global alias.cl clone

git config --global alias.st "status -sb"
git config --global alias.last "log -1 --stat"
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %Cblue<%an>%Creset' --abbrev-commit --date=relative --all"
git config --global alias.dc "diff --cached"

git config --global alias.unstage "reset HEAD --"

git config --global alias.ci commit
git config --global alias.ca "commit -a"

git config --global alias.ri "rebase -i"
git config --global alias.ria "rebase -i --autosquash"
git config --global alias.fix "commit --fixup"
git config --global alias.squ "commit --squash"

git config --global alias.cp cherry-pick
git config --global alias.co checkout
git config --global alias.br branch
git config --global core.editor emacs
~~~~

## Emacs

Instalado emacs desde los repos:

~~~~
sudo aptitude install emacs
~~~~

* Configuramos la fuente por defecto del editor y salvamos las
  opciones. Con esto generamos el fichero `~/.emacs`
* __Importante__: Configuramos la _face_ para la _region_ con un color
  que nos guste. Parece que viene configurado por defecto igual que el
  texto normal y nunca veremos la _region_ resaltada aunque queramos.
* Editamos el fichero `.emacs` y añadimos los depósitos de paquetes
  (nunca he conseguido que _Marmalade_ funcione)

Esta es la sección donde configuramos los depósitos de paquetes:

~~~~
;;----------------------------------------------------------------------
;; MELPA and others
(when (>= emacs-major-version 24)
  (require 'package)
  (package-initialize)
  (add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)
  (add-to-list 'package-archives '("gnu" . "http://elpa.gnu.org/packages/") t)
;;  (add-to-list 'package-archives '("marmalade" . "https://marmalade-repo.org/packages/") t)
  )
~~~~

GNU Elpa es el depósito oficial, tiene menos paquetes y son todos con
licencia FSF.

Melpa y Marmalade son paquetes de terceros. Tienen mucha más variedad
pero con calidades dispares.

Desde Melpa con el menú de gestión de paquetes de emacs, instalamos
los siguientes paquetes^[Mantenemos aquí la lista de paquetes
instalados en emacs aunque no todos son de desarrollo software]:

* _markdown-mode_
* _pandoc-mode_
* _auto-complete_
* _ac-dcd_
* _d-mode_
* _flycheck_
* _flycheck-dmd-dub_
* _flycheck-d-unittest_
* _elpy_
* _jedi_
* _auctex-latexmk_
* _py_autopep8_
* _auctex_
* _smartparens_
* _yasnippets_ (se instala como dependencia)


Después de probar _flymake_ y _flycheck_ al final me ha gustado más
_flycheck_ Hay una sección de configuración en el fichero `.emacs` para
cada uno de ellos, pero la de _flymake_ está comentada.

Configuramos el fichero `.emacs` definimos algunas preferencias,
algunas funciones útiles y añadimos orígenes extra de paquetes.

~~~~
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(show-paren-mode t))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )

;;------------------------------------------------------------
;; Some settings
(setq inhibit-startup-message t) ; Eliminate FSF startup msg
(setq frame-title-format "%b")   ; Put filename in titlebar
;(setq visible-bell t)           ; Flash instead of beep
(set-scroll-bar-mode 'right)     ; Scrollbar placement
(show-paren-mode t)              ; Blinking cursor shows matching parentheses
(setq column-number-mode t)      ; Show column number of current cursor location
(mouse-wheel-mode t)             ; wheel-mouse support

(setq fill-column 78)
(setq auto-fill-mode t)          ; Set line width to 78 columns...

(setq-default indent-tabs-mode nil)       ; Insert spaces instead of tabs
(global-set-key "\r" 'newline-and-indent) ; turn autoindenting on
;(set-default 'truncate-lines t)          ; Truncate lines for all buffers
;(require 'iso-transl)                    ; doesn't seems to be needed in debian


;;------------------------------------------------------------
;; Some useful key definitions
(define-key global-map [M-S-down-mouse-3] 'imenu)
(global-set-key [C-tab] 'hippie-expand)                    ; expand
(global-set-key [C-kp-subtract] 'undo)                     ; [Undo]
(global-set-key [C-kp-multiply] 'goto-line)                ; goto line
(global-set-key [C-kp-add] 'toggle-truncate-lines)         ; goto line
(global-set-key [C-kp-divide] 'delete-trailing-whitespace) ; delete trailing whitespace
(global-set-key [C-kp-decimal] 'completion-at-point)       ; complete at point
(global-set-key [C-M-prior] 'next-buffer)                  ; next-buffer
(global-set-key [C-M-next] 'previous-buffer)               ; previous-buffer

;;------------------------------------------------------------
;; Set encoding
(prefer-coding-system 'utf-8)
(setq coding-system-for-read 'utf-8)
(setq coding-system-for-write 'utf-8)

;;------------------------------------------------------------
;; Maximum colors
(cond ((fboundp 'global-font-lock-mode)        ; Turn on font-lock (syntax highlighting)
       (global-font-lock-mode t)               ; in all modes that support it
       (setq font-lock-maximum-decoration t))) ; Maximum colors

;;------------------------------------------------------------
;; Use % to match various kinds of brackets...
;; See: http://www.lifl.fr/~hodique/uploads/Perso/patches.el

(global-set-key "%" 'match-paren)               ; % key match parents
(defun match-paren (arg)
  "Go to the matching paren if on a paren; otherwise insert %."
  (interactive "p")
  (let ((prev-char (char-to-string (preceding-char)))
        (next-char (char-to-string (following-char))))
    (cond ((string-match "[[{(<]" next-char) (forward-sexp 1))
          ((string-match "[\]})>]" prev-char) (backward-sexp 1))
          (t (self-insert-command (or arg 1))))))

;;------------------------------------------------------------
;; The wonderful bubble-buffer
(defvar LIMIT 1)
(defvar time 0)
(defvar mylist nil)

(defun time-now ()
   (car (cdr (current-time))))

(defun bubble-buffer ()
   (interactive)
   (if (or (> (- (time-now) time) LIMIT) (null mylist))
       (progn (setq mylist (copy-alist (buffer-list)))
          (delq (get-buffer " *Minibuf-0*") mylist)
          (delq (get-buffer " *Minibuf-1*") mylist)))
   (bury-buffer (car mylist))
   (setq mylist (cdr mylist))
   (setq newtop (car mylist))
   (switch-to-buffer (car mylist))
   (setq rest (cdr (copy-alist mylist)))
   (while rest
     (bury-buffer (car rest))
     (setq rest (cdr rest)))
   (setq time (time-now)))

(global-set-key [f8] 'bubble-buffer)    ; win-tab switch the buffer

(defun geosoft-kill-buffer ()
   ;; Kill default buffer without the extra emacs questions
   (interactive)
   (kill-buffer (buffer-name))
   (set-name))
(global-set-key [C-delete] 'geosoft-kill-buffer)

;;----------------------------------------------------------------------
;; MELPA and others
(when (>= emacs-major-version 24)
  (require 'package)
  (package-initialize)
  (add-to-list 'package-archives '("melpa" . "http://melpa.org/packages/") t)
  (add-to-list 'package-archives '("gnu" . "http://elpa.gnu.org/packages/") t)
  (add-to-list 'package-archives '("marmalade" . "https://marmalade-repo.org/packages/") t)
  )

; (add-to-list 'load-path "~/.emacs.d/")

;;----------------------------------------------------------------------
;; Packages installed via package
;;------------------------------

;;----------------------------------------------------------------------
;; flymake and flycheck installed from package
;; I think you have to choose only one

;; (require 'flymake)
;; ;;(global-set-key (kbd "C-c d") 'flymake-display-err-menu-for-current-line)
;; (global-set-key (kbd "C-c d") 'flymake-popup-current-error-menu)
;; (global-set-key (kbd "C-c n") 'flymake-goto-next-error)
;; (global-set-key (kbd "C-c p") 'flymake-goto-prev-error)

(add-hook 'after-init-hook #'global-flycheck-mode)
(global-set-key  (kbd "C-c C-p") 'flycheck-previous-error)
(global-set-key  (kbd "C-c C-n") 'flycheck-next-error)

;; Define d-mode addons
;; Activate flymake or flycheck for D
;; Activate auto-complete-mode
;; Activate yasnippet minor mode if available
;; Activate dcd-server
(require 'ac-dcd)
(add-hook 'd-mode-hook
          (lambda()
            ;;(flymake-d-load)
            (flycheck-dmd-dub-set-variables)
            (require 'flycheck-d-unittest)
            (setup-flycheck-d-unittest)
            (auto-complete-mode t)
            (when (featurep 'yasnippet)
              (yas-minor-mode-on))
            (ac-dcd-maybe-start-server)
            (ac-dcd-add-imports)
            (add-to-list 'ac-sources 'ac-source-dcd)
            (define-key d-mode-map (kbd "C-c ?") 'ac-dcd-show-ddoc-with-buffer)
            (define-key d-mode-map (kbd "C-c .") 'ac-dcd-goto-definition)
            (define-key d-mode-map (kbd "C-c ,") 'ac-dcd-goto-def-pop-marker)
            (define-key d-mode-map (kbd "C-c s") 'ac-dcd-search-symbol)
            (when (featurep 'popwin)
              (add-to-list 'popwin:special-display-config
                           `(,ac-dcd-error-buffer-name :noselect t))
              (add-to-list 'popwin:special-display-config
                           `(,ac-dcd-document-buffer-name :position right :width 80))
              (add-to-list 'popwin:special-display-config
                           `(,ac-dcd-search-symbol-buffer-name :position bottom :width 5)))))

;; Define diet template mode (this is not installed from package)
(add-to-list 'auto-mode-alist '("\\.dt$" . whitespace-mode))
(add-hook 'whitespace-mode-hook
          (lambda()
            (setq tab-width 2)
            (setq whitespace-line-column 250)
            (setq indent-tabs-mode nil)
            (setq indent-line-function 'insert-tab)))

;;----------------------------------------------------------------------
;; elpy
(elpy-enable)
~~~~

## Lenguaje de programación D (D programming language)

El lenguaje de programación D es un lenguaje de programación de
sistemas con una sintaxis similar a la de C y con tipado estático.
Combina eficiencia, control y potencia de modelado con seguridad y
productividad.

### D-apt e instalación de programas

Configurado _d-apt_, instalados todos los programas incluidos

~~~~
sudo wget http://master.dl.sourceforge.net/project/d-apt/files/d-apt.list -O /etc/apt/sources.list.d/d-apt.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys  EBCF975E5BA24D5E
sudo apt update
~~~~

Instalamos todos los programas asociados excepto _textadept_ que falla por problemas de librerias.

~~~~
sudo apt install dmd-compiler dmd-tools dub dcd dfix dfmt dscanner
~~~~

### DCD

Una vez instalado el DCD tenemos que configurarlo creando el fichero
`~/.config/dcd/dcd.conf` con el siguiente contenido:

~~~~
/usr/include/dmd/druntime/import
/usr/include/dmd/phobos
~~~~

Podemos probarlo con:

~~~~
dcd-server &
echo | dcd-client --search toImpl
~~~~

### gdc

Instalado con:

~~~~
sudo aptitude install gdc
~~~~

### ldc

Instalado con:

~~~~
sudo aptitude install ldc
~~~~

Para poder ejecutar aplicaciones basadas en Vibed, necesitamos instalar:

~~~~
sudo apt-get install -y libssl-dev libevent-dev
~~~~

### Emacs para editar D

Instalados los siguientes paquetes desde Melpa

* d-mode
* flymake-d
* flycheck
* flycheck-dmd-dub
* flychek-d-unittest
* auto-complete (desde melpa)
* ac-dcd

Referencias
* (https://github.com/atilaneves/ac-dcd)
* (https://github.com/Hackerpilot/DCD)

## Processing

Bajamos los paquetes de las respectivas páginas web, descomprimimimos
en `~/apps/`, en las nuevas versiones incorpora un script de
instalación que ya se encarga de crear el fichero _desktop_.

## Python

De partida tenemos instalado dos versiones: _python_ y _python3_

~~~~{bash}
python -V
Python 2.7.12

python3 -V
Python 3.5.2
~~~~

### Paquetes de desarrollo

Para que no haya problemas a la hora de instalar paquetes en el futuro
conviene que instalemos los paquetes de desarrollo:

~~~~
sudo apt install python-dev
sudo apt install python3-dev
~~~~

### pip, virtualenv, virtualenvwrapper, virtualfish

Los he instalado a nivel de sistema.

_pip_ es un gestor de paquetes para __Python__ que facilita la
instalación de librerías y utilidades.

Para poder usar los entornos virtuales instalaremos también
_virtualenv_.

Instalamos los dos desde aptitude:

~~~~{bash}
sudo apt install python-pip python-virtualenv virtualenv python3-pip
~~~~

_virtualenv_ es una herramienta imprescindible en Python, pero da un
poco de trabajo, así que se han desarrollado algunos frontends para
simplificar su uso, para _bash_ y _zsh_ usaremos _virtualenvwrapper_,
y para _fish_ el _virtualfish_. Como veremos son todos muy parecidos.

Instalamos el virtualwrapper:

~~~~{bash}
sudo apt install virtualenvwrapper -y
~~~~

Para usar _virtualenvwrapper_ tenemos que hacer:

~~~~{bash}
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
~~~~

O añadir esa linea a nuestros ficheros _.bashrc_ y/o _.zshrc_

Definimos la variable de entorno *WORKON_HOME* para que
apunte al directorio por defecto, `~/.local/share/virtualenvs`. En ese directorio
es donde se guardarán nuestros entornos virtuales.

En el fichero `.profile` añadimos:

~~~~
# WORKON_HOME for virtualenvwrapper
if [ -d "$HOME/.local/share/virtualenvs" ] ; then
    WORKON_HOME="$HOME/.local/share/virtualenvs"
fi
~~~~

[Aquí](http://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html)
tenemos la referencia de comandos de _virtualenvwrapper_

Por último, si queremos tener utilidades parecidas en nuestro _fish
shell_ instalamos _virtualfish_:

~~~~{bash}
sudo pip install virtualfish
~~~~

[Aquí](http://virtualfish.readthedocs.io/en/latest/index.html) tenemos
la documentación de _virtualfish_ y la descripción de todos los
comandos y plugins disponibles.

### pipenv

No lo he instalado, pero en caso de instalación mejor lo instalamos a
nivel de usuario con:

~~~~{bash}
pip install --user pipenv
~~~~


### Instalación de bpython y ptpython

[_bpython_](https://bpython-interpreter.org/) instalado desde repos `sudo apt install bpython bpython3`

[_ptpython_](https://github.com/prompt-toolkit/ptpython) instalado en un virtualenv para probarlo


### Emacs para programar python

Para instalar `elpy`

~~~~
sudo apt install python-jedi python3-jedi
# flake8 for code checks
sudo apt install flake8 python-flake8 python3-flake8
# and autopep8 for automatic PEP8 formatting
sudo apt install python-autopep8
# and yapf for code formatting
sudo apt install yapf yapf3
~~~~


Añadimos la sección

~~~~{lisp}
;;----------------------------------------------------------------------
;; elpy
(elpy-enable)
(setq elpy-rpc-backend "jedi")

(add-hook 'python-mode-hook 'jedi:setup)
(setq jedi:complete-on-dot t)
~~~~

Desde _Emacs_ ejecutamos: `alt-x jedi:install-server`

#### TODO

Estudiar esto con calma <https://elpy.readthedocs.io/en/latest>

### Jupyter

Una instalación para pruebas.

~~~~
mkvirtualenv -p /usr/bin/python3 jupyter
python -m pip install jupyter 
~~~~


## neovim

Vamos a probar _neovim_:

~~~~
sudo apt-add-repository ppa:neovim-ppa/stable
sudo apt-get update
sudo apt-get install neovim
~~~~

Para instalar los módulos de python ^[aun no lo hice]:

~~~~
sudo pip install --upgrade neovim
sudo pip3 install --upgrade neovim
~~~~

Revisar [esto](https://neovim.io/doc/user/provider.html#provider-python)

Para actualizar las alternativas:

~~~~
sudo update-alternatives --install /usr/bin/vi vi /usr/bin/nvim 60
sudo update-alternatives --config vi
sudo update-alternatives --install /usr/bin/vim vim /usr/bin/nvim 60
sudo update-alternatives --config vim
~~~~

#### Install _vim-plug_

Ejecutamos:

~~~~
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
~~~~

Configuramos el fichero de configuración de _nvim_
(`~/.config/nvim/init.vim`):

~~~~
" Specify a directory for plugins
" - For Neovim: ~/.local/share/nvim/plugged
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.local/share/nvim/plugged')

if has('nvim')
  Plug 'Shougo/deoplete.nvim', { 'do': ':UpdateRemotePlugins' }
else
  Plug 'Shougo/deoplete.nvim'
  Plug 'roxma/nvim-yarp'
  Plug 'roxma/vim-hug-neovim-rpc'
endif
let g:deoplete#enable_at_startup = 1


Plug 'zchee/deoplete-jedi'

" Initialize plugin system
call plug#end()
~~~~

La primera vez que abramos _nvim_ tenemos que instalar los plugin por
comando ejecutando: `:PlugInstall`

__Instalación de `dein`__

Solo hay que instalar uno de los dos o _dein_ o _plug-vim_. Yo uso
_plug-vim_ así que esto es sólo una referencia.

<https://github.com/Shougo/dein.vim>

~~~~
" Add the dein installation directory into runtimepath
set runtimepath+=~/.config/nvim/dein/repos/github.com/Shougo/dein.vim

if dein#load_state('~/.config/nvim/dein')
  call dein#begin('~/.config/nvim/dein')

  call dein#add('~/.config/nvim/dein/repos/github.com/Shougo/dein.vim')
  call dein#add('Shougo/deoplete.nvim')
  call dein#add('Shougo/denite.nvim')
  if !has('nvim')
    call dein#add('roxma/nvim-yarp')
    call dein#add('roxma/vim-hug-neovim-rpc')
  endif

  call dein#end()
  call dein#save_state()
endif

filetype plugin indent on
syntax enable
~~~~

## Firefox developer edition

El rollo de siempre, descargar desde [la página
web](https://www.mozilla.org/en-US/firefox/developer/) descomprimir en
`~/apps` y crear un lanzador.


## MariaDB

Instalamos la última estable desde los repos oficiales:

~~~~{bash}
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.icm.edu.pl/pub/unix/database/mariadb/repo/10.4/ubuntu bionic main'

sudo apt update
sudo apt install mariadb-server
~~~~

Falla al poner _password_ al usuario `root` así que tendremos que ponerla manualmente:

Paramos el servicio con `sudo service mariadb stop`

Arrancamos sin comprobación de usuarios

~~~~
sudo mysqld_safe --skip-grant-tables --skip-networking &

mariadb -u root
~~~~

Una vez en el _prompt_ de la base de datos ejecutamos:

~~~~
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
FLUSH PRIVILEGES;
~~~~

Ahora podemos reiniciar el ordenador y el usuario `root` ya tendrá la
contraseña correcta en la base de datos.
