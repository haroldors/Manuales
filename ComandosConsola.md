--
## SSH ##
## el siguiente comando es para conectrase con ssh a la maquina virtual de brainiac ##
´´´
ssh -i ~/.ssh/my-ssh-key uservmfromlinux@104.198.178.252
´´´
## Git ##
## el siguiente comando permite registrar el usuario y password de git para que no lo solicite nuevamente ##
´´´
git config --global credential.helper store
´´´
## el siguiente comando permite corregir el problema de acceso a realizar commit en Git ##
este es el problema que muestra la pantalla
´´´
Insufficient permission for adding an object to repository database .git/objects.
´´´
esta es la solucion aplicada con mi usuario brainiac en mi repositorio manuales
´´´
cd .git/objects
ls -al
sudo chown -R brainiac:brainiac *
ls -al
´´´
