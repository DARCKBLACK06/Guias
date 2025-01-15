# Minecraft Server

### flatpak run com.mojang.Minecraft
---

## **Paso 1: Actualizar el sistema**
Actualiza los paquetes instalados en tu sistema para garantizar que todo esté al día:
```bash
sudo dnf update -y
```

---

## **Paso 2: Instalar Java**
Minecraft requiere Java para ejecutarse. Se recomienda instalar OpenJDK 17 (compatible con las últimas versiones del servidor de Minecraft):

```bash
sudo dnf install -y java-17-openjdk
```

Verifica la instalación de Java:
```bash
java -version
```
Deberías ver algo como:
```plaintext
openjdk version "17.x.x"
```

## **Paso 3: Descargar archivo del servidor**
```
https://launcher.mojang.com/v1/objects/952438ac4e01b4d115c5fc38f891710c4941df29/server.jar
https://files.minecraftforge.net/net/minecraftforge/forge/
```

## **Paso 4: Ejecutar el archivo servidor por primera vez**
```
java -Xmx1024M -Xms1024M -jar server.jar
```

## **Paso 5: Habilitar archivo eula.txt**
```
#By changing the setting below to TRUE you are indicating your agreement to our EULA (https://account.mojang.com/documents/minecraft_eula).
#Sun Nov 24 12:45:30 CST 2024
eula=true
```

## **Paso 6: Configurar archivo server.properties**
```
#Minecraft server properties
#Sun Nov 24 12:56:08 CST 2024
view-distance=10
max-build-height=256
server-ip=
level-seed=
allow-nether=true
enable-command-block=false
server-port=25565
gamemode=0
enable-rcon=false
op-permission-level=4
enable-query=false
generator-settings=
resource-pack=
player-idle-timeout=0
level-name=world
motd=IRD43
announce-player-achievements=true
force-gamemode=false
hardcore=false
white-list=false
pvp=true
spawn-npcs=true
generate-structures=true
spawn-animals=true
snooper-enabled=true
difficulty=1
level-type=DEFAULT
spawn-monsters=true
max-players=300
spawn-protection=16
online-mode=false
allow-flight=false
```

## **Paso 7: Ejecutar servidor**
```
java -Xmx1024M -Xms1024M -jar server.jar
```

## **Paso 8: Abrir el puerto 25565 en el firewall**
Permite las conexiones al servidor de Minecraft:
```bash
sudo firewall-cmd --permanent --add-port=25565/tcp
sudo firewall-cmd --reload
```

---
