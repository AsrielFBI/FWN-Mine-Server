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
```
Una vez ya ejecutados estes comandos, añadir la siguiente configuración para que cada vez que se dispare el evento push, jenkins actualize la config y reinicie el server

## Comandos para cada evento de push
```bash
if screen -S MineServ -X select . ; then
	/usr/bin/screen -p 0 -S MineServ -X eval 'stuff "bc GitHub Update | Reinicio en 10 segundos"\015'
    sleep 10s
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "save-all"\015'
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "stop"\015'
    sleep 20s
    echo "REUTIL"
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "cd $WORKSPACE"\015'
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "java -Xms1G -Xmx5G -jar spigot-1.16.1.jar -o true | lolcat"\015'
    return 0
else
    /usr/bin/screen -dmS MineServ sh
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "cd $WORKSPACE"\015'
    /usr/bin/screen -p 0 -S MineServ -X eval 'stuff "java -Xms1G -Xmx5G -jar spigot-1.16.1.jar -o true | lolcat"\015'
    return 0
fi
```

> Directorio de archivos --> `/var/lib/docker/volumes/MineJenkins/_data/worlkspaces/mineserv`
