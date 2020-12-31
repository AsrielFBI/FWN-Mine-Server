# FWN-Mine-Server
Servidor minecraft FAE / Prototipo

Por favor, no robar sin permiso.




Servidor creado y mantenido por RubenPX y AsrielFBI

I cannot understand Spanish - DLG_Protogen 2020
(Originally made for a high-end computer server)

# Configuración de jenkins
Contenedor usado: `jenkins/jenkins`
Los siguientes comandos son para instalar todo lo necesario en la maquina
```bash
apt install screen rsync lolcat
ln /usr/games/lolcat /bin/lolcat

mkdir /var/MineServ
chown jenkins:jenkins /var/MineServ
```
Una vez ya ejecutados estes comandos, añadir la siguiente configuración para que cada vez que se dispare el evento push, jenkins actualize la config y reinicie el server

## Comandos para cada evento de push
```bash
if screen -S MineServ -X select . ; then
	/usr/bin/screen -p 0 -S MineServ -X eval 'stuff "msg @a GitHub Update | Reinicio en 10 segundos"\015'
    sleep 10s
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "save-all"\015'
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "stop"\015'
    sleep 10
    rsync -av "$WORKSPACE/" /var/MineServ/
    echo "REUTIL"
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "cd /var/MineServ/"\015'
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "bash /var/MineServ/runlinux.sh"\015'
    return 0
else
    rsync -av "$WORKSPACE/" /var/MineServ/
    /usr/bin/screen -dmS MineServ sh
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "cd /var/MineServ/"\015'
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "bash /var/MineServ/runlinux.sh"\015'
    return 0
fi
```

