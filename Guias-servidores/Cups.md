# Reporte: Instalación y Configuración de CUPS en Linux

## Introducción

CUPS (Common Unix Printing System) es el sistema de impresión utilizado en la mayoría de las distribuciones Linux, permitiendo la gestión de impresoras locales y en red. En este documento, se describe paso a paso la instalación y configuración de CUPS, además de la apertura de los puertos necesarios en el firewall para permitir la administración remota.

---

## Instalación de CUPS

Para instalar CUPS en el sistema, utilizamos el siguiente comando:

```bash
sudo dnf install cups cups-pdf
```

Este comando descarga e instala los paquetes necesarios para el servicio de impresión.

---

## Habilitación y Arranque del Servicio

Una vez instalado, activamos e iniciamos el servicio con los siguientes comandos:

```bash
sudo systemctl enable --now cups
sudo systemctl start cups
```

Estos comandos permiten que CUPS se inicie automáticamente en cada arranque del sistema y garantizan que el servicio esté en ejecución.

---

## Modificación de la Configuración de CUPS

Los archivos de configuración de CUPS se encuentran en `/etc/cups/`. Accedemos a esta ubicación con:

```bash
cd /etc/cups
```

Luego, editamos el archivo de configuración `cupsd.conf` usando un editor de texto como `vim`:

```bash
vim cupsd.conf
```

En este archivo, realizamos las siguientes modificaciones:

1. **Permitir conexiones remotas**: Eliminamos la restricción de `localhost` para permitir accesos desde otras direcciones IP.
   ```
   Listen 631
   Listen /run/cups/cups.sock
   ```
2. **Configurar permisos de acceso**: Agregamos la directiva `Allow all` para permitir el acceso general:
   ```
   <Location />
     Allow all
     Order allow,deny
   </Location>

   <Location /admin>
     Allow all
     AuthType Default
     Require user @SYSTEM
     Order allow,deny
   </Location>

   <Location /admin/conf>
     Allow all
     AuthType Default
     Require user @SYSTEM
     Order allow,deny
   </Location>

   <Location /admin/log>
     Allow all
     AuthType Default
     Require user @SYSTEM
     Order allow,deny
   </Location>
   ```

---

## Reinicio del Servicio

Después de modificar la configuración, reiniciamos el servicio de CUPS para aplicar los cambios:

```bash
sudo systemctl restart cups
```

---

## Configuración del Firewall

Para permitir la comunicación con CUPS, abrimos los puertos necesarios en el firewall:

```bash
firewall-cmd --add-service=ipp-client --permanent
sudo firewall-cmd --add-port=631/tcp --permanent
firewall-cmd --reload
```

---

## Acceso a la Interfaz Web de CUPS

Para administrar CUPS desde una interfaz web, accedemos a la siguiente URL en un navegador:

```
http://<IP_DEL_SERVIDOR>:631
```

Por ejemplo:

```
http://50.7.12.15:631
```

Desde aquí podemos añadir y gestionar impresoras de manera gráfica.
![Descripción de la imagen](https://github.com/DARCKBLACK06/Guias/blob/main/Capturas/MenuCups)

---

## Configuración Adicional para Compartir Impresoras

Si deseamos permitir la administración remota y compartir impresoras en la red, ejecutamos:

```bash
cupsctl --remote-admin --remote-any --share-printers
```

Este comando habilita la administración remota, permite accesos desde cualquier dirección y comparte las impresoras configuradas en CUPS.

---

## Conclusión

Con estos pasos, hemos instalado y configurado CUPS para gestionar impresoras en un entorno Linux. También hemos habilitado la administración remota y configurado el firewall para permitir la comunicación adecuada. Ahora, podemos acceder a la interfaz web de CUPS para agregar y administrar impresoras fácilmente.


