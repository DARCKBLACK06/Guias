# Configuración de Samba en AlmaLinux

## Instalación de Samba
Para instalar Samba en AlmaLinux, ejecute el siguiente comando:
```bash
dnf install samba samba-common samba-client
```

## Habilitar y arrancar los servicios de Samba
```bash
systemctl enable --now smb nmb
systemctl start --now smb nmb
```

## Configurar el firewall para permitir Samba
```bash
firewall-cmd --permanent --zone=public --add-service=samba
firewall-cmd --reload
```

## Verificar la instalación
Para verificar que los archivos de configuración de Samba están presentes:
```bash
cd /etc/samba/
ls
```

## Creación de grupo y usuario para compartir archivos
```bash
groupadd servidores
useradd -m javier
passwd javier
usermod -G Servidores javier
```

## Crear el directorio compartido y asignar permisos
```bash
mkdir netsecure
chown :servidores -R netsecure/
chmod 770 -R netsecure/
```

## Agregar usuario a Samba
```bash
smbpasswd -a javier
```

## Configurar el firewall para permitir Samba completamente
```bash
firewall-cmd --permanent --add-service={samba,samba-client,samba-dc} --zone=public
firewall-cmd --reload
```

## Configuración del archivo `smb.conf`
Edite el archivo de configuración de Samba:
```bash
vi /etc/samba/smb.conf
```

Agregue o modifique las siguientes líneas:
```ini
[global]
        workgroup = WORKGROUP
        security = user
        map to guest = never

[Servidores]
        path = /home/netsecure
        valid users = javier
        read only = no
        browseable = yes
        writable = yes
        guest ok = yes
        create mask = 0770
```

## Reiniciar los servicios de Samba
Para aplicar los cambios, reinicie los servicios:
```bash
systemctl restart smb nmb
```

## Verificar el estado del servicio
Para comprobar que Samba está corriendo sin errores:
```bash
systemctl status smb nmb
```
