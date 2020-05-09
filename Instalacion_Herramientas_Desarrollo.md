# Instalacion de Herramientas de Programacion en LinuxMint 19.1

---
---

## 1. Instalacion de Sublime-text 3
---
### Para instalar
* a continuacion se indican los comando para la instalacion de sublime text

```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```
* Para aplicar el tema Adaptive:
    * se debe seleccionar Preferencias
    * Luego Adaptive.sublime-theme

* Para instalar control de paquetes
    * se debe hacer click en tool
    * luego en install package controll

* Para Instalar el esquema Monakai Dark
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

* Para Instalar puglin que permite minificar archivos css y js utilizaremos el puglin Minify
    * primero debemos ejecutar por consola los siguientes comandos:
    ```
            apt install nodejs npm cleancss uglifyjs

            npm update -g clean-css-cli uglifycss js-beautify html-minifier uglify-js minjson svgo

    ```
    * lueego en Sublime Text se debe hacer click en preferencias
    * luego en package controll
    * luego seleccionar **Package Control:** Install Package
    * luego se debe seleccionar Minify
    * luego debe ir a preferences
    * luego debe ir a Package Settings
    * luego debe ir a Minify -> Setting Users
    * en la seccion usuario que sale en el costado derecho debe agregar lo siguiente:
    ```
            {
            "auto_minify_on_save": true
            }
    ``` 
    * con estos parametros permitiremos, que cada vez que se guarde el archivo automticamente genere en paraleo un archivo minificado que es el que publicaremos despues.

* Para corregir e problema de generar comentarios y asignar la tecla / del teclado numerico derecho, debemos hacer lo siguiente
    * primero debemos hacer click en Preferences
    * Luego hacer click en Key Bindings y en la seccion usuario que sale en el costado derecho debemos agregar lo siguiente:
    ```
            { "keys": ["ctrl+shift+keypad_divide"], "command": "toggle_comment", "args": { "block": true } }
    ```


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

### Para Trabajar con archivos Markdown (utiles para desarrollo de manuales)

* primero agregaremos los siguientes plugin
    * **Markdown Preview Github Styling** el cual nos permitira visualizar el el costado derecho como queda finalmente 
    * **Markdown PDF** nos permitira exportar el archivo markdown a varios formatos (pdf,html,jpeg,png)
        * Para utilziar este puglin una vez instalado solo debemos precionar Ctrl+Shift+P y selecionar Markdown PDF: export(all:pdf,html,png,jpeg)
        * lo anterior nos generara en la misma carpeta del archivo .md los arhivos con los formatos nuevos.

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
