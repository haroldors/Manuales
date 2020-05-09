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
