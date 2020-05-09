# Instalacion de Herramientas de Programacion en LinuxMint 19.1

---
---

## 1. Instalacion de Sublime-text 3

### Para instalar
* a continuacion se indican los comando para la instalacion de sublime text

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```
* Para aplicar el tema:
    * se debe seleccionar Preferencias
    * Luego Adaptive.sublime-theme

* Para instalar control de paquetes
    * se debe hacerclick en tool
    * luego en install package controll

* Para Instalar Monaki Dark
    * se debe hacer click en prerefencias
    * luego en package controll
    * luego seleccionar **Package Control:** Install Package
    * luego se debe seleccionar Monokay Dark para que se instale
    * luego hacer click en preferencias
    * luego hacer click en **color escheme...**
    * luego selecionar Monokay Dark y con esto se aplicara

* Para instalar el puglin que elimina los espacios vacios
    * se debe hacer click en preferencias
    * luego en package controll
    * luego seleccionar **Package Control:** Install Package
    * luego se debe seleccionar Trailingspaces
    * luego debe ir a preferences
    * luego debe ir a Package Settings
    * luego debe ir a Trailing spaces -> Setting
    * en la seccion usuario que sale en el costado derecho debe agregar lo siguiente:
    
    ```
            "trailing_spaces_trim_on_save": true
    ```

* Para instalar puglin que modifica los iconos de los archivos
    * se debe hacer click en preferencias
    * luego en package controll
    * luego seleccionar **Package Control:** Install Package
    * luego se debe seleccionar FileIcons



---
## 2. Instalacion de Visual Studio Code

* a continuacion se indican lo comandos para la instalacion de Visual Studio Code

```
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /usr/share/keyrings/
sudo sh -c 'echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install code
```
### Para cambiar el idioma 

* se debe precionar ctrl+shift+P y seleccionar configurar idioma de pantalla

* Luego se debe seleccionar el idioma que queremos

* una vez instalado nos solicitara reiniciar el editor y luego aparecera el idioma

---

## 3. Instalacion de herramienta de Administracion de Base de datos dbeaver

* a continuacion se indican los comandos para la instalacion de dbeaver

```
sudo add-apt-repository ppa:serge-rider/dbeaver-ce
sudo apt-get update
sudo apt-get install dbeaver-ce
``` 

---
## 4. Instalacion de Anydesk para soporte remote

* se deje ejecutar los siguientes comandos

```
wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | apt-key add -
echo "deb http://deb.anydesk.com/ all main" > /etc/apt/sources.list.d/anydesk-stable.list
apt update
apt install anydesk
```
