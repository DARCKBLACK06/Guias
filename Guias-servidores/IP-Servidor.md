### Paso 1: Verifica la interfaz de red
Primero, identifica el nombre de la interfaz de red con el comando:

```bash
ip a
```

Esto mostrará todas las interfaces de red. Busca la interfaz de red activa, como `eth0`, `ens33`, o similar.

#### Editar el archivo de configuración manualmente

En **AlmaLinux** y otras distribuciones de Linux basadas en **RHEL (Red Hat Enterprise Linux)**, el comando `ifcfg` se usaba tradicionalmente para configurar interfaces de red en versiones más antiguas de RHEL/CentOS. Sin embargo, en versiones recientes y en AlmaLinux 8 en adelante, la configuración de red ha migrado a **NetworkManager**, y el archivo de configuración `ifcfg-*` aún existe pero se usa de forma algo distinta y en combinación con `nmcli` o `nmtui`.

### 1. Ubicación de los Archivos `ifcfg`
Los archivos de configuración de red todavía se encuentran en:
```bash
/etc/sysconfig/network-scripts/
```

Cada archivo `ifcfg-*` corresponde a una interfaz de red específica. Por ejemplo, `ifcfg-eth0` correspondería a la interfaz `eth0`.

### 2. Ejemplo de un Archivo de Configuración `ifcfg`
Un archivo típico `ifcfg` para configurar una interfaz estática se vería así:
```ini
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
DNS2=8.8.4.4
```

### 3. Consultar el Estado de las Interfaces
Para verificar el estado de una interfaz:
```bash
nmcli device status
```

### 4. Aplicar Cambios Usando `nmcli`
Para aplicar los cambios de configuración de red, usa `nmcli`, el cliente de línea de comandos de NetworkManager:
```bash
nmcli connection reload
nmcli connection up eth0
```
